# AXUMORTEM

### Static Binary Analysis Engine for Malware Triage & Threat Intelligence

<p align="center">
  <img src="./assets/axumortem-ui.jpg" alt="AXUMORTEM Dashboard" width="100%">
</p>

<p align="center">
  <strong>Dissecting binaries without execution.</strong><br>
  ELF • PE • Mach-O • YARA • Entropy • Disassembly • MITRE ATT&CK
</p>

---

## Overview

AXUMORTEM is a modern static malware analysis platform designed to inspect executable files without executing them.

The platform enables security researchers, malware analysts, incident responders, and reverse engineers to quickly assess suspicious binaries through automated static analysis techniques.

Instead of running potentially dangerous files, AXUMORTEM extracts intelligence directly from binary structure, imports, strings, entropy patterns, disassembly, and malware signatures.

---

## Dashboard Preview

<p align="center">
  <img src="./assets/axumortem-ui.jpg" alt="AXUMORTEM Interface" width="100%">
</p>

The interface provides a streamlined workflow for uploading binaries, running static analysis, and reviewing threat intelligence results in real time.

---

## Core Features

### Binary Parsing
- Windows PE
- Linux ELF
- macOS Mach-O

### YARA Scanning
- Malware signature matching
- Rule-based threat detection
- Family identification

### Entropy Analysis
- Detect packed binaries
- Identify encrypted sections
- Highlight obfuscation techniques

### Disassembly
- Function extraction
- API analysis
- Instruction inspection

### String Analysis
- ASCII / UTF-8 / Unicode extraction
- IOC discovery
- Suspicious artifact detection

### MITRE ATT&CK Mapping
- Technique correlation
- Threat scoring
- Analyst-friendly reporting

---

## Architecture

```text
Frontend (Next.js)
        │
        ▼
Backend API (Rust + Axum)
        │
 ┌──────┼──────┐
 ▼      ▼      ▼
Parser YARA Disassembler
        │
        ▼
 PostgreSQL
```

## Technology Stack

### Backend
- Rust
- Axum
- Tokio
- SQLx
- PostgreSQL
- YARA

### Frontend
- Next.js
- TypeScript
- Tailwind CSS

### Infrastructure
- Docker
- Docker Compose

---

## Local Setup

```bash
git clone https://github.com/YOUR_USERNAME/axumortem.git
cd axumortem
docker compose up -d
```

Open:

```text
http://localhost:22784
```

---

## Future Enhancements

- Dynamic Sandbox Analysis
- VirusTotal Integration
- IOC Extraction
- Memory Analysis
- AI-Powered Classification

---

## License

MIT License

---

<p align="center">
Built for Reverse Engineers, Malware Analysts, and Security Researchers.
</p>