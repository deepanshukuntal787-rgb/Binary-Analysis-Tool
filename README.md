# 🦠 Axumortem: Advanced Static Binary Analysis Engine

## Table of Contents
1. [Project Overview](#project-overview)
2. [Core Capabilities](#core-capabilities)
3. [System Architecture](#system-architecture)
4. [Technical Stack](#technical-stack)
5. [In-Depth Feature Breakdown](#in-depth-feature-breakdown)
6. [Local Setup & Deployment](#local-setup--deployment)
7. [Learning Modules](#learning-modules)

---

## 1. Project Overview
**Axumortem** is a highly scalable, high-performance static binary analysis engine. Its primary purpose is to dissect compiled executables (without executing them) to identify malware, packed payloads, security vulnerabilities, and structural anomalies. 

Built on a modern stack featuring **Rust** for heavy computational tasks and **React** for a sleek, data-rich user interface, Axumortem automates the reverse-engineering pipeline. It applies a multi-pass analysis system to parse headers, extract imports/exports, scan for known malware signatures using YARA, and disassemble machine code.

---

## 2. Core Capabilities

- **Cross-Platform Binary Parsing:** Capable of parsing Linux (ELF), Windows (PE), and macOS (Mach-O) executable formats.
- **YARA Signature Scanning:** Integrates `yara-x` to scan binaries against 14 built-in, industry-standard detection rules for ransomware, trojans, and crypto-miners.
- **x86/x86_64 Disassembly:** Utilizes `iced-x86` to translate raw machine code into human-readable assembly instructions, generating Control Flow Graphs (CFGs).
- **Entropy Analysis:** Calculates Shannon entropy across binary sections to detect encrypted or compressed (packed) payloads often used by malware authors to evade detection.
- **MITRE ATT&CK Threat Scoring:** Maps discovered anomalies to the MITRE ATT&CK framework, generating a 100-point threat score to quickly assess the danger level of a file.

---

## 3. System Architecture

The project is structured as a distributed micro-architecture, containerized using Docker.

```text
┌─────────────────────────┐
│     User Interface      │
│     (React / Vite)      │
└────────────┬────────────┘
             │ (HTTP / JSON)
             ▼
┌─────────────────────────┐
│       Nginx Proxy       │
│     (Port: 22784)       │
└────────────┬────────────┘
             │ (Reverse Proxy)
             ▼
┌─────────────────────────┐      ┌─────────────────────────┐
│   Backend API (Axum)    │─────▶│  PostgreSQL 18 (Data)   │
│     (Port: 3000)        │      └─────────────────────────┘
└────────────┬────────────┘
             │
      ┌──────┴──────┐
      ▼             ▼
┌───────────┐ ┌───────────┐
│ YARA Scan │ │ Disasm /  │
│  Engine   │ │ Entropy   │
└───────────┘ └───────────┘
```

### Data Flow
1. **Upload:** A user uploads a binary file via the React frontend.
2. **Routing:** Nginx proxies the upload to the Rust backend API.
3. **Pipeline Execution:** The Rust backend executes a topological analysis pipeline:
   - **Pass 1:** Format parsing (identifying if it's ELF, PE, etc.).
   - **Pass 2:** Extraction of strings, imports, and exports.
   - **Pass 3:** Heavy computation (YARA scanning, Entropy calculation, Disassembly).
4. **Storage:** Results are aggregated, scored, and stored in the PostgreSQL database.
5. **Presentation:** The frontend fetches the analysis report and renders it in an interactive dashboard.

---

## 4. Technical Stack

### Backend
- **Language:** Rust (chosen for memory safety and extreme performance)
- **Framework:** Axum (high-performance web framework)
- **Binary Parsing:** `goblin`
- **Disassembly:** `iced-x86`
- **Signature Scanning:** `yara-x`
- **Database ORM:** SQLx

### Frontend
- **Framework:** React 19 (TypeScript)
- **Build Tool:** Vite
- **State Management:** Zustand (global state) & TanStack Query (server state/caching)
- **Styling:** SCSS Modules

### Infrastructure
- **Database:** PostgreSQL 18
- **Proxy:** Nginx
- **Containerization:** Docker & Docker Compose
- **Command Runner:** Just (`justfile`)

---

## 5. In-Depth Feature Breakdown

### A. Format Parsing & Section Analysis
When a binary is uploaded, the engine breaks it down into its core structural components:
- **Headers:** Extracts target architecture, entry points, and compiler metadata.
- **Sections:** Analyzes `.text` (code), `.data` (initialized data), and `.bss` (uninitialized data) sections.
- **Import/Export Tables:** Identifies external DLLs/libraries the program relies on. (e.g., Identifying a dependency on `ws2_32.dll` on Windows indicates network activity capabilities).

### B. Shannon Entropy Analysis
Malware authors frequently compress ("pack") or encrypt their code to hide it from antivirus software. Axumortem calculates the **Shannon Entropy** (a measure of randomness from 0 to 8) for every section of the binary. 
- A section with an entropy **> 7.2** is mathematically highly random, heavily implying encryption or packing.

### C. YARA Scanning
YARA is the industry standard for pattern matching. Axumortem runs the binary against a database of rules containing byte-patterns and strings associated with known malware families. If a rule triggers, the threat score increases significantly.

### D. MITRE ATT&CK Threat Scoring
Rather than just providing raw data, the engine interprets the results. It assigns a risk score (0-100) based on 8 categories. For example, if the engine detects anti-debugging tricks or network-socket imports, it maps these to specific MITRE ATT&CK tactics (like *Defense Evasion* or *Command and Control*) and elevates the threat score.

---

## 6. Local Setup & Deployment

### Prerequisites
- Docker Desktop
- `git`

### Quick Start (Production Mode)
```bash
# 1. Clone the repository
git clone https://github.com/deepanshukuntal787-rgb/Binary-Analysis-Tool.git
cd Binary-Analysis-Tool

# 2. Configure Environment Variables
cp .env.example .env

# 3. Spin up the containers
docker compose up -d
```
Visit `http://localhost:22784` in your browser.

### Development Mode (Hot-Reloading)
To run the project locally while actively developing code:
```bash
docker compose -f dev.compose.yml up -d
```

---

## 7. Learning Modules
Axumortem was built with education in mind. The repository contains detailed guides breaking down the cybersecurity theory and software architecture behind the engine:

- `learn/00-OVERVIEW.md` - Setup and intro
- `learn/01-CONCEPTS.md` - Reverse engineering and malware analysis theory
- `learn/02-ARCHITECTURE.md` - Rust backend design patterns
- `learn/03-IMPLEMENTATION.md` - Code walkthrough
- `learn/04-CHALLENGES.md` - Exercises for extending the engine

---
*Developed by Deepanshu Kuntal*
