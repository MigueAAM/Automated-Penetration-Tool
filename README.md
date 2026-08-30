<div align="center">
  <h1>Automated Penetration Testing Orchestrator</h1>
  <p><strong>A modular Breach and Attack Simulation (BAS) pipeline for advanced threat modeling.</strong></p>

  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/asyncio-black?style=for-the-badge&logo=python&logoColor=white" alt="asyncio" />
  <img src="https://img.shields.io/badge/Nmap-4E46CE?style=for-the-badge&logo=nmap&logoColor=white" alt="Nmap" />
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js" />
</div>

<br />

## Project Overview
The Automated Penetration Testing Orchestrator is a decoupled cybersecurity platform designed to simulate a professional red team assessment workflow. Unlike standard standalone execution tools, this system operates as a continuous programmatic pipeline where the structured output of one module dynamically serves as the automated input for the next. It systematically discovers networked assets, identifies vulnerable services, safely validates exploits, and compiles findings into standardized reports.

## Core Architecture & Engineering Concepts
*   **Asynchronous Reconnaissance:** Leverages Python's `asyncio` and Nmap integration to perform high-speed, non-blocking service discovery and banner grabbing without exhausting system resources.
*   **Vulnerability Correlation via CPE:** Programmatically maps raw version strings into strict Common Platform Enumeration (CPE) formats to ensure highly accurate querying against the NIST NVD REST API 2.0 for real-time CVE identifiers and CVSS risk scores.
*   **Safe Execution Engine:** Prioritizes enterprise safety by utilizing non-destructive proof-of-concept (PoC) checks over destructive payloads, preventing accidental Denial of Service (DoS) while securely confirming exploitability.

## The 5-Phase Pipeline
| Phase | Status | Technical Implementation |
| :--- | :--- | :--- |
| **1. Reconnaissance** | 🟢 Completed | Enforces strict Rules of Engagement (RoE) boundaries and extracts machine-readable service fingerprints using async network probes and Nmap. |
| **2. Correlation** | 🟡 Active | Ingests JSON scan data and retrieves active CVE identifiers via the NVD database API using `virtualMatchString`. |
| **3. Validation** | ⚪ Pending | A dynamic plugin registry that parses CVEs and routes them to safe Python PoC scripts for non-destructive verification against Docker targets. |
| **4. Reporting** | ⚪ Pending | Translates aggregated JSON data into NIST/CIS compliant executive summaries and technical remediation steps. |
| **5. Dashboard** | ⚪ Pending | A React/Next.js frontend interface bridging the Python orchestrator for live progress tracking and risk visualization. |

## Directory Structure
```text
auto_pentest_platform/
├── phase1_recon/           # Scope validation & Nmap async scanning
├── phase2_correlation/     # NVD API CVE formatting
├── phase3_validation/      # Safe PoC execution engine
├── data/                   # Dynamic artifact JSON storage
├── config.py               # Global path resolution
└── run_pipeline.py         # Master CLI Orchestrator
