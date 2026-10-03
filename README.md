<div align="center">
  <h1>Heaven's Door</h1>
  <p><strong>Write Down Our Target's History!</strong></p>

  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/asyncio-black?style=for-the-badge&logo=python&logoColor=white" alt="asyncio" />
  <img src="https://img.shields.io/badge/Nmap-4E46CE?style=for-the-badge&logo=nmap&logoColor=white" alt="Nmap" />
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js" />
</div>

<br />

## Project Overview
Heaven's Door is an Automated Penetration Testing Orchestrator is a decoupled cybersecurity platform designed for professional red team assessment workflow. Unlike standard standalone execution tools, this system operates as a continuous programmatic pipeline where the structured output of one module dynamically serves as the automated input for the next. It systematically discovers networked assets, identifies vulnerable services, safely validates exploits, and compiles findings into standardized reports.

## Core Architecture & Engineering Concepts
*   **Asynchronous Reconnaissance:** Leverages Python's `asyncio` and Nmap integration to perform high-speed, non-blocking service discovery and banner grabbing without exhausting system resources.
*   **Vulnerability Correlation via CPE:** Programmatically maps raw version strings into strict Common Platform Enumeration (CPE) formats to ensure highly accurate querying against the NIST NVD REST API 2.0 for real-time CVE identifiers and CVSS risk scores.
*   **Safe Execution Engine:** Prioritizes enterprise safety by utilizing non-destructive proof-of-concept (PoC) checks over destructive payloads, preventing accidental Denial of Service (DoS) while securely confirming exploitability.

# Execution & Deployment Commands:

## 1. Test Environment Initialization
To safely validate the pipeline without exposing production networks, initialize the local target lab bound to the loopback interface (`127.0.0.1`)

```bash
cd test_target
docker compose up --build -d
```

## 2. Master Orchestrator Execution
```bash
python run_pipeline.py --target 127.0.0.1
```

# Modular Phase Execution (CLI)
The pipeline features a decoupled architecture where each phase can be executed independently, reading from and writing to the central `data/` directory.
## Phase 1 - Reconnaissance & Scope Guard: 
Discovers open ports and extracts service banners using Nmap, validating targets against authorized CIDRs.
```bash
py -3 -m phase1_recon.main --target 127.0.0.1
```

## Phase 2 - Vulnerability Correlation:
Translates Phase 1 findings into CPE 2.3 syntax and queries the NIST NVD API. Custom inputs and outputs can be specified using `--file` and `--output` flags.
```bash
py -3 -m phase2_correlation.correlator
```

## Phase 3 - Safe Validation Engine:
Deduplicates targets and dynamically executes safe Proof-of-Concept checks for correlated CVEs.
```bash
py -3 -m phase3_validation.main
```

## Phase 4 - Report Generation:
Ingests the JSON artifacts from prior phases to render compliance-mapped Markdown and HTML reports.
```bash
py -3 -m phase4_reporting.renderer
```

## The 5-Phase Pipeline
| Phase | Status | Technical Implementation |
| :--- | :--- | :--- |
| **1. Reconnaissance** | 🟢 Completed | Enforces strict Rules of Engagement (RoE) boundaries and extracts machine-readable service fingerprints using async network probes and Nmap. |
| **2. Correlation** | 🟢 Completed | Ingests JSON scan data and retrieves active CVE identifiers via the NVD database API using `virtualMatchString`. |
| **3. Validation** | 🟢 Completed | A dynamic plugin registry that parses CVEs and routes them to safe Python PoC scripts for non-destructive verification against Docker targets. |
| **4. Reporting** | 🟡 Active - Development | Translates aggregated JSON data into NIST/CIS compliant executive summaries and technical remediation steps. |
| **5. Dashboard** | ⚪ Pending | A React/Next.js frontend interface bridging the Python orchestrator for live progress tracking and risk visualization. |

## Directory Structure
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
