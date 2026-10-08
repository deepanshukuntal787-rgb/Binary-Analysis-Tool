# AXUMORTEM
### Static Binary Analysis Engine for Malware Triage & Threat Intelligence

<p align="center">
  <img src="./assets/banner.png" alt="AXUMORTEM Banner" width="100%">
</p>

<p align="center">
  <strong>Dissecting binaries without execution.</strong><br>
  ELF • PE • Mach-O • YARA • Entropy • Disassembly • MITRE ATT&CK
</p>

---

## Overview

AXUMORTEM is a modern static malware analysis platform designed to inspect executable files without executing them.

The platform enables security researchers, malware analysts, incident responders, and reverse engineers to quickly assess suspicious binaries through automated static analysis techniques.

Instead of running potentially dangerous files, AXUMORTEM extracts intelligence directly from the binary structure, imports, strings, entropy patterns, disassembly, and malware signatures.

---

## Why AXUMORTEM?

Traditional malware analysis often requires sandbox execution, virtualization, and behavioral monitoring.

AXUMORTEM provides a safer first layer of defense by:

- Identifying suspicious indicators before execution
- Detecting known malware signatures
- Highlighting obfuscation and packing techniques
- Mapping behaviors to MITRE ATT&CK tactics
- Providing rapid triage for incident response teams

---

# Core Capabilities

## Cross-Platform Binary Parsing

AXUMORTEM supports analysis of:

- Windows PE Executables
- Linux ELF Binaries
- macOS Mach-O Files

The engine automatically detects file type and extracts:

- Headers
- Sections
- Imports
- Exports
- Symbols
- Entry Points
- Metadata

---

## YARA Signature Scanning

The engine scans uploaded binaries against curated YARA rulesets.

### Benefits

- Detect known malware families
- Identify ransomware samples
- Recognize trojans and loaders
- Flag suspicious indicators

### Example

```yara
rule Suspicious_Powershell
{
    strings:
        $ps = "powershell.exe"
    condition:
        $ps
}
```

When matched, AXUMORTEM reports:

- Rule Name
- Severity
- Description
- Match Location

---

## Disassembly Analysis

The platform performs static disassembly to reveal low-level program behavior.

### Extracted Information

- Functions
- Control Flow
- API Calls
- Instructions
- Suspicious Opcodes

This enables analysts to understand:

- Persistence mechanisms
- Credential theft routines
- Process injection attempts
- Network communication logic

---

## String Extraction

Embedded strings frequently reveal attacker intent.

AXUMORTEM extracts:

- ASCII Strings
- UTF-8 Strings
- Unicode Strings

Examples:

```text
cmd.exe
powershell.exe
CreateRemoteThread
VirtualAllocEx
```

These indicators help identify:

- Command execution
- Injection techniques
- C2 communication
- Persistence behavior

---

## Entropy Analysis

Entropy measures randomness within binary sections.

High entropy often indicates:

- Packed executables
- Encrypted payloads
- Obfuscated malware

### Shannon Entropy Scale

| Entropy | Interpretation |
|----------|---------------|
| 0 – 4 | Normal |
| 4 – 6 | Moderate |
| 6 – 8 | Suspicious |
| > 7.5 | Likely Packed / Encrypted |

The platform visualizes entropy per section to quickly identify anomalies.

---

## MITRE ATT&CK Threat Scoring

AXUMORTEM maps discovered indicators to MITRE ATT&CK techniques.

Example:

| Indicator | ATT&CK Technique |
|------------|----------------|
| PowerShell Execution | T1059.001 |
| Registry Persistence | T1547 |
| Process Injection | T1055 |
| Credential Dumping | T1003 |

This helps analysts understand attacker behavior patterns in a standardized framework.

---

# System Architecture

```text
                    ┌─────────────────────┐
                    │     Frontend UI     │
                    │      Next.js        │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     REST API        │
                    │      Rust/Axum      │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼

 ┌─────────────┐     ┌────────────────┐     ┌──────────────┐
 │ File Parser │     │ YARA Scanner   │     │ Disassembler │
 └─────────────┘     └────────────────┘     └──────────────┘
          │                    │                    │
          └──────────┬─────────┴─────────┬──────────┘
                     ▼                   ▼

           ┌───────────────────────┐
           │ Analysis Aggregator   │
           └──────────┬────────────┘
                      ▼

           ┌───────────────────────┐
           │ PostgreSQL Database   │
           └──────────┬────────────┘
                      ▼

           ┌───────────────────────┐
           │ Threat Intelligence   │
           │ & Reporting Engine    │
           └───────────────────────┘
```

---

# Analysis Workflow

## Step 1 — Upload Binary

User uploads:

```text
sample.exe
sample.elf
sample.macho
```

---

## Step 2 — File Identification

The engine determines:

- File Type
- Architecture
- Compiler Information
- Metadata

---

## Step 3 — Static Extraction

AXUMORTEM extracts:

- Imports
- Exports
- Strings
- Sections
- Headers

---

## Step 4 — YARA Scanning

The binary is scanned against malware signatures.

---

## Step 5 — Entropy Analysis

Each section is analyzed for:

- Packing
- Encryption
- Obfuscation

---

## Step 6 — Disassembly

Machine code is converted into human-readable assembly instructions.

---

## Step 7 — Threat Scoring

Indicators are mapped to:

- MITRE ATT&CK
- Risk Categories
- Threat Levels

---

## Step 8 — Results Dashboard

The frontend presents:

- Binary Overview
- Imports
- Strings
- Entropy Graphs
- YARA Hits
- Threat Score

---

# Technology Stack

## Backend

- Rust
- Axum
- Tokio
- SQLx
- PostgreSQL
- Serde
- YARA

---

## Frontend

- Next.js
- TypeScript
- TailwindCSS
- React Query

---

## Infrastructure

- Docker
- Docker Compose
- PostgreSQL
- Linux Containers

---

# Project Structure

```text
axumortem/
│
├── backend/
│   ├── src/
│   ├── handlers/
│   ├── services/
│   ├── analysis/
│   └── Cargo.toml
│
├── frontend/
│   ├── app/
│   ├── components/
│   ├── lib/
│   └── package.json
│
├── docker/
│
├── compose.yml
│
└── README.md
```

---

# Local Setup

## Production Environment

Clone the repository:

```bash
git clone https://github.com/yourusername/axumortem.git
cd axumortem
```

Start containers:

```bash
docker compose up -d
```

Verify services:

```bash
docker ps
```

Open:

```text
http://localhost:22784
```

---

## Development Environment

Start with logs:

```bash
docker compose up
```

Stop:

```bash
docker compose down
```

Restart:

```bash
docker compose restart
```

---

# Future Enhancements

- Dynamic Sandbox Analysis
- Memory Dump Inspection
- VirusTotal Integration
- Sigma Rule Correlation
- IOC Extraction
- Threat Hunting Dashboard
- AI-Powered Malware Classification
- Graph-Based Attack Visualization

---

# Security Notice

AXUMORTEM performs static analysis only.

Do not execute untrusted binaries outside controlled environments.

Always use isolated systems when handling malware samples.

---

# License

MIT License

---

<p align="center">
Built for Reverse Engineers, Malware Analysts, and Security Researchers.
</p>