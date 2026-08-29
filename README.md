<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Automated Penetration Testing Orchestrator</title>
    <style>
        :root {
            --bg-primary: #0d1117;
            --bg-secondary: #161b22;
            --text-primary: #c9d1d9;
            --text-secondary: #8b949e;
            --accent: #58a6ff;
            --border: #30363d;
            --success: #3fb950;
            --warning: #d29922;
        }
        
        * { box-sizing: border-box; }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            background-color: var(--bg-primary);
            color: var(--text-primary);
            line-height: 1.6;
            margin: 0;
            padding: 3rem 1rem;
            display: flex;
            justify-content: center;
        }
        
        .container { 
            max-width: 850px; 
            width: 100%; 
        }
        
        h1, h2, h3 { 
            color: #ffffff; 
            border-bottom: 1px solid var(--border); 
            padding-bottom: 0.3em; 
            margin-top: 2rem; 
        }
        
        h1 { 
            font-size: 2.5rem; 
            text-align: center; 
            border: none; 
            margin-bottom: 0.2em; 
            margin-top: 0; 
        }
        
        .subtitle { 
            text-align: center; 
            font-size: 1.1rem; 
            color: var(--text-secondary); 
            margin-bottom: 2.5rem; 
        }
        
        .badges { 
            display: flex; 
            justify-content: center; 
            gap: 10px; 
            margin-bottom: 2.5rem; 
            flex-wrap: wrap; 
        }
        
        .badge { 
            background: var(--bg-secondary); 
            border: 1px solid var(--border); 
            padding: 5px 14px; 
            border-radius: 2em; 
            font-size: 0.85rem; 
            font-weight: 600; 
            color: var(--accent); 
        }
        
        p { margin-bottom: 1rem; }
        
        .card { 
            background-color: var(--bg-secondary); 
            border: 1px solid var(--border); 
            border-radius: 8px; 
            padding: 1.5rem 2rem; 
            margin-top: 1.5rem; 
        }
        
        table { 
            width: 100%; 
            border-collapse: collapse; 
            margin-top: 1.5rem; 
            font-size: 0.95rem; 
        }
        
        th, td { 
            padding: 14px; 
            text-align: left; 
            border-bottom: 1px solid var(--border); 
        }
        
        th { 
            background-color: rgba(255,255,255,0.03); 
            color: #fff; 
        }
        
        .status-completed { color: var(--success); font-weight: 600; }
        .status-active { color: var(--warning); font-weight: 600; }
        .status-pending { color: var(--text-secondary); font-weight: 600; }
        
        ul { padding-left: 20px; }
        li { margin-bottom: 0.75rem; }
        
        code { 
            background-color: rgba(110,118,129,0.4); 
            padding: 0.2em 0.4em; 
            border-radius: 6px; 
            font-family: ui-monospace, SFMono-Regular, Consolas, monospace; 
            font-size: 0.85em; 
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Automated Penetration Testing Orchestrator</h1>
        <div class="subtitle">A modular Breach and Attack Simulation (BAS) pipeline for advanced threat modeling.</div>
        
        <div class="badges">
            <span class="badge">Python 3.9+</span>
            <span class="badge">asyncio</span>
            <span class="badge">Nmap</span>
            <span class="badge">Next.js</span>
        </div>

        <div class="card">
            <h2>Project Overview</h2>
            <p>The Automated Penetration Testing Orchestrator is a decoupled cybersecurity platform designed to simulate a professional red team assessment workflow. Unlike standard standalone execution tools, this system operates as a continuous programmatic pipeline where the structured output of one module dynamically serves as the automated input for the next[cite: 2]. It systematically discovers networked assets, identifies vulnerable services, safely validates exploits, and compiles findings into standardized reports[cite: 2].</p>
        </div>

        <h2>Core Architecture & Engineering Concepts</h2>
        <ul>
            <li><strong>Asynchronous Reconnaissance:</strong> Leverages Python's <code>asyncio</code> and Nmap integration to perform high-speed, non-blocking service discovery and banner grabbing without exhausting system resources.</li>
            <li><strong>Vulnerability Correlation via CPE:</strong> Programmatically maps raw version strings into strict Common Platform Enumeration (CPE) formats to ensure highly accurate querying against the NIST NVD REST API 2.0 for real-time CVE identifiers and CVSS risk scores[cite: 2, 6].</li>
            <li><strong>Safe Execution Engine:</strong> Prioritizes enterprise safety by utilizing non-destructive proof-of-concept (PoC) checks over destructive payloads, preventing accidental Denial of Service (DoS) while securely confirming exploitability[cite: 2, 6].</li>
        </ul>

        <h2>The 5-Phase Pipeline</h2>
        <table>
            <thead>
                <tr>
                    <th>Phase</th>
                    <th>Status</th>
                    <th>Technical Implementation</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><strong>1. Reconnaissance</strong></td>
                    <td class="status-completed">Completed</td>
                    <td>Enforces strict Rules of Engagement (RoE) boundaries and extracts machine-readable service fingerprints using async network probes[cite: 2, 6].</td>
                </tr>
                <tr>
                    <td><strong>2. Correlation</strong></td>
                    <td class="status-completed">Completed</td>
                    <td>Ingests JSON scan data and retrieves active CVE identifiers via the NVD database API[cite: 2, 6].</td>
                </tr>
                <tr>
                    <td><strong>3. Validation</strong></td>
                    <td class="status-active">Active</td>
                    <td>A dynamic plugin registry that parses CVEs and routes them to safe Python PoC scripts for non-destructive verification[cite: 1, 2].</td>
                </tr>
                <tr>
                    <td><strong>4. Reporting</strong></td>
                    <td class="status-pending">Pending</td>
                    <td>Translates aggregated JSON data into NIST/CIS compliant executive summaries and technical remediation steps[cite: 2, 6].</td>
                </tr>
                <tr>
                    <td><strong>5. Dashboard</strong></td>
                    <td class="status-pending">Pending</td>
                    <td>A React/Next.js frontend interface bridging the Python orchestrator for live progress tracking and risk visualization[cite: 2, 6].</td>
                </tr>
            </tbody>
        </table>
    </div>
</body>
</html>
