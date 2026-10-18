# Privacy Pipeline: GDPR Auto-PII Redaction
 
A user-accessible web tool that detects and redacts personally identifiable information (PII) from text, CSV, JSON and log data, then produces an audit-ready GDPR compliance report. It runs entirely in the browser. No data is uploaded to any server.
 
> **Live demo:** _add your Netlify / GitHub Pages link here_
 
![Screenshot](docs/screenshot.png)
 
## Features
 
- **One-click pipeline** with five visible stages: Ingest, Detect, Classify, Redact, Audit
- **14 PII types** detected: email, person name, phone, credit card, SSN, Aadhaar, PAN, IBAN, passport number, date of birth, IP address, street address, API key or token, MAC address
- **Checksum validation** to cut false positives
  - Credit cards: Luhn algorithm
  - Aadhaar: Verhoeff algorithm
  - IBAN: mod-97 check
- **Column-aware CSV handling**: columns headed `name`, `full_name`, `first_name`, `last_name` or `surname` are redacted even without labels
- **Four redaction modes**
  - Typed tag, e.g. `[EMAIL]`
  - Full mask, `[REDACTED]`
  - Pseudonymisation with salted SHA-256 (the same value always maps to the same pseudonym)
  - Partial mask that keeps the last 4 characters
- **Per-type toggles** to choose exactly which PII types to redact
- **Verification re-scan** of the output to report any residual PII
- **Compliance report**
  - Records scanned, entities found, distinct types, residual count
  - Entities-by-type chart and an overall risk score (Low, Medium or High)
  - Audit log with line number, type, GDPR basis, confidence, replacement and a salted fingerprint
  - Print or save as PDF
- Fluent-style UI with light and dark themes, scroll reveal animations and a responsive layout
## Getting started
 
No build step and no dependencies.
 
```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
# open index.html in any modern browser
```
 
Rename `pii-redaction.html` to `index.html` if you want to host it on GitHub Pages or Netlify.
 
## Usage
 
1. Click **Load sample data**, paste your own text, or **Upload file** (`.txt`, `.csv`, `.json`, `.log`, `.tsv`).
2. Choose a redaction mode and, optionally, switch PII types on or off with the chips.
3. Click **Run pipeline**.
4. Review the highlighted input, the redacted output and the generated report. Use **Copy** for the redacted text and **Print / save as PDF** for the report.
## How detection works
 
| Layer | Used for |
|---|---|
| Regular expressions | Email, phone, SSN, PAN, passport, DOB, IP, MAC, API keys and tokens, addresses |
| Checksum validators | Credit card (Luhn), Aadhaar (Verhoeff), IBAN (mod-97) |
| Label and context heuristics | Person names after labels such as `Name:`, `Mr.`, `Dr.` or JSON keys like `"name"` |
| Column detection | Name columns in CSV files |
| Overlap resolution | The earliest and longest match wins when patterns overlap |
 
Each audit entry carries a confidence rating. Checksum-validated and strongly structured types are rated **High**. Names, addresses, phone numbers and IPs are rated **Medium** because they are pattern-based.
 
## GDPR mapping
 
| Reference | Meaning in this tool |
|---|---|
| Art. 4(1) | Personal data: names, email, phone, address, DOB |
| Art. 5(1)(c) and Art. 25 | Data minimisation and privacy by design through early redaction |
| Art. 32 and Art. 4(5) | Security and pseudonymisation through salted hashing |
| Art. 87 | National identification numbers |
| Recital 30 | Online identifiers such as IP and MAC addresses |
 
## Limitations
 
- Detection is rule-based. Names in free-text sentences with no label or column header can be missed.
- Phone, address and name matching is heuristic, so review the output before sharing sensitive data.
- Inputs are capped at roughly 300,000 characters when uploaded as a file.
- This tool is a technical aid and **not legal advice**. It does not by itself make a system GDPR compliant.
## Roadmap
 
- [ ] FastAPI backend with spaCy Named Entity Recognition for better name, organisation and location detection
- [ ] Kafka-based streaming ingestion
- [ ] PostgreSQL audit logging and Grafana compliance dashboards
- [ ] Docker Compose setup for the full pipeline
- [ ] Downloadable redacted file and JSON report export
## Tech stack
 
HTML, CSS and vanilla JavaScript (Web Crypto API for SHA-256). The roadmap stack is Python, FastAPI, spaCy, Apache Kafka, PostgreSQL, Docker and Grafana.
 
## Project structure
 
```
.
├── index.html     # the entire app (UI, detection engine, report)
├── README.md
└── docs/
    └── screenshot.png
```
 
## Author
 
**Kaviyarasu V**
[LinkedIn](http://www.linkedin.com/in/kaviyarasu-v) · [Portfolio](https://portfolio-kaviyarasu.netlify.app/)
 
## License
 
MIT. Add a `LICENSE` file to your repository before publishing.
 
Claude finished the response
