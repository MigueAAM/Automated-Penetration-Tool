<div align="center">
  <h1>Heaven's Door</h1>
  <p><strong>Write Down Our Target's History!</strong></p>

  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/asyncio-black?style=for-the-badge&logo=python&logoColor=white" alt="asyncio" />
  <img src="https://img.shields.io/badge/Nmap-4E46CE?style=for-the-badge&logo=nmap&logoColor=white" alt="Nmap" />
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js" />
</div>

<br />

# Project Overview
Heaven's Door is an Automated Penetration Testing Orchestrator is a decoupled cybersecurity platform designed for professional red team assessment workflow. Unlike standard standalone execution tools, this system operates as a continuous programmatic pipeline where the structured output of one module dynamically serves as the automated input for the next. It systematically discovers networked assets, identifies vulnerable services, safely validates exploits, and compiles findings into standardized reports.

# Core Architecture & Engineering Concepts
*   **Asynchronous Reconnaissance:** Leverages Python's `asyncio` and Nmap integration to perform high-speed, non-blocking service discovery and banner grabbing without exhausting system resources.
*   **Vulnerability Correlation via CPE:** Programmatically maps raw version strings into strict Common Platform Enumeration (CPE) formats to ensure highly accurate querying against the NIST NVD REST API 2.0 for real-time CVE identifiers and CVSS risk scores.
*   **Safe Execution Engine:** Prioritizes enterprise safety by utilizing non-destructive proof-of-concept (PoC) checks over destructive payloads, preventing accidental Denial of Service (DoS) while securely confirming exploitability.

# The 5-Phase Pipeline
| Phase | Status | Technical Implementation |
| :--- | :--- | :--- |
| **1. Reconnaissance** | 🟢 Completed | Enforces strict Rules of Engagement (RoE) boundaries and extracts machine-readable service fingerprints using async network probes and Nmap. |
| **2. Correlation** | 🟢 Completed | Ingests JSON scan data and retrieves active CVE identifiers via the NVD database API using `virtualMatchString`. |
| **3. Validation** | 🟢 Completed | A dynamic plugin registry that parses CVEs and routes them to safe Python PoC scripts for non-destructive verification against Docker targets. |
| **4. Reporting** | 🟡 Testing | Translates aggregated JSON data into NIST/CIS compliant executive summaries and technical remediation steps. |
| **5. Dashboard** | ⚪ Pending | A React/Next.js frontend interface bridging the Python orchestrator for live progress tracking and risk visualization. |

# Directory Structure
```text
auto_pentest_platform/
├── phase1_recon/           # Scope validation & Nmap async scanning
|    ├── main.py
|    ├── models.py
|    ├── scanner.py
|    ├── scope_guard.py
├── phase2_correlation/     # NVD API CVE formatting
|    ├── Correlator.py
├── phase3_validation/      # Safe PoC execution engine
|    ├── CVE_Scripts/
|    |    ├── base.py
|    |    ├── cve_2021_42013.py
|    |    ├── cve_2021_41773.py
|    |    ├── cve_2023_23934.py
|    ├── engine.py
|    ├── loader.py
|    ├── main.py
|    ├── poc_loader.py
├── phase4_reporting/       # Report engine using templates
|    ├── Templates/
├── phase5_dashboard/       # Interactive Dashboard
|    ├── Backend/
|    ├── Frontend/
├── data/                   # Dynamic artifact JSON storage
|    ├── Final_Reports/
|    ├── Scanner_Results/
|    ├── Validation_Results/
|    ├── Vulnerability_Results/
├── config.py               # Global path resolution
└── run_pipeline.py         # Master CLI Orchestrator
```

# Heaven's Door Pipeline

Heaven's Door runs the reconnaissance, vulnerability correlation, safe
validation, and reporting phases as one command-line pipeline.

## Run the complete pipeline

Run the pipeline from the project root:

```powershell
python run_pipeline.py --target ip_address
```

To restrict Phase 1 to specific TCP ports, provide a comma-separated list:

```powershell
python run_pipeline.py --target ip_address --ports 
```

The target must be inside one of Phase 1's authorized CIDR ranges unless the
phase is configured with a different scope.

## Artifact naming

The pipeline derives one filesystem-safe stem from `--target`. Periods and
colons are replaced with underscores. For example, `192.168.2.10` becomes
`192_168_2_10`.

Each phase receives the exact artifact path produced by the previous phase:

| Phase | Artifact | Default location |
| --- | --- | --- |
| Phase 1: reconnaissance | `192_168_2_10_scan.json` | `data/scanner_results/` |
| Phase 2: correlation | `192_168_2_10_correlation.json` | `data/vulnerability_results/` |
| Phase 3: validation | `192_168_2_10_validation.json` | `data/validation_results/` |
| Phase 4: Markdown report | `192_168_2_10_report.md` | `data/final_reports/` |
| Phase 4: HTML report | `192_168_2_10_report.html` | `data/final_reports/` |

The shared `pipeline_paths()` helper in `config.py` defines these names. The
orchestrator passes explicit paths so a phase cannot accidentally select a
stale file based on modification time.

## Phase 1: reconnaissance

Run Phase 1 independently:

```powershell
python -m phase1_recon.main --target 
```

Important arguments:

- `--target` is required.
- `--ports` optionally limits the scan, for example `--ports 5000,8080`.
- `--output` optionally supplies an exact JSON output path. Without it, the
  output uses the target-derived `<target>_scan.json` name.
- `--allowed-cidrs` optionally replaces the default authorized CIDR list.

Phase 1 performs service detection and writes the scan result as JSON.

## Phase 2: vulnerability correlation

Run Phase 2 with an exact Phase 1 artifact:

```powershell
python -m phase2_correlation.correlator `
  --file data/scanner_results/ip_address_scan.json `
  --output data/vulnerability_results/ip_address_correlation.json
```

Important arguments:

- `--file` identifies the scanner JSON to read. It may be an absolute path,
  a relative path, or a filename in `data/scanner_results/`.
- `--output` identifies the correlation JSON to write. It may be an absolute
  path, a relative path, or a filename in `data/vulnerability_results/`.
- `--target` supplies target metadata when the scanner JSON does not contain
  an address.

The complete pipeline always supplies both paths explicitly. This avoids the
old mismatch between Phase 1's target-derived output and Phase 2's generic
`scan_result.json` default.

## Phase 4: reporting

Run Phase 4 with the exact artifacts from Phases 1–3:

```powershell
python -m phase4_reporting.renderer `
  --scan-file data/scanner_results/ip_address_scan.json `
  --correlation-file data/vulnerability_results/ip_address_correlation.json `
  --validation-file data/validation_results/ip_address_validation.json `
  --output-stem ip_address
```

Important arguments:

- `--scan-file` selects the Phase 1 scan artifact.
- `--correlation-file` selects the Phase 2 correlation artifact.
- `--validation-file` selects the Phase 3 validation artifact.
- `--output-stem` controls the `<stem>_report.md` and
  `<stem>_report.html` filenames.

When these arguments are omitted, Phase 4 retains standalone fallback
behavior: it discovers the newest Phase 1 and Phase 2 JSON files and uses the
standard validation result location. The complete pipeline passes every path
explicitly and therefore does not use fallback discovery.

## Pipeline handoff

The intended handoff is:

```text
--target
   |
   v
Phase 1 -> <target>_scan.json
   |
   v
Phase 2 -> <target>_correlation.json
   |
   v
Phase 3 -> <target>_validation.json
   |
   v
Phase 4 -> <target>_report.md
       -> <target>_report.html
```

Use `run_pipeline.py` for normal operation so all phases process artifacts
from the same target and run.
