# IT360-Group-Project

**Phishing Email Forensics Analyzer**

Tyler and Sam - IT 360 Final Project

> This is a working README. The project is in active development, so features, structure, and instructions will change as we build.

---

## Overview

A command-line tool that analyzes suspicious email files and produces a structured forensic report.

Phishing is one of the most common ways attackers get into a network, and the evidence that proves it is sitting in the message itself — the headers, the links, the attachments. Our tool pulls that evidence out automatically and gives an analyst a clear summary instead of a raw email file.

## What It Does

- Traces the message's delivery path through its headers to find where it really came from
- Checks sender authentication (SPF, DKIM, DMARC) and flags failures
- Detects sender spoofing and lookalike domains
- Extracts and defangs all URLs so they're safe to review
- Hashes attachments and identifies their real file types
- Exports indicators of compromise to JSON and CSV
- Uses an AI model to write a plain-language summary of the tactics used in the message
- Preserves forensic integrity: read-only access, evidence hashing, and an audit log

## Repository Structure

```
IT360-Group-Project/
├── src/                  # Tool source code
├── docs/                 # Written report and supporting images
├── data/                 # Synthetic sample emails
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Setup

*Instructions will be finalized once the tool is complete.*

```bash
git clone https://github.com/<user>/IT360-Group-Project.git
cd IT360-Group-Project
pip install -r requirements.txt
```

## Usage

*Usage will be finalized once the tool is complete.*

```bash
python src/main.py --input data/sample_phish.eml --output reports/
```

## Sample Evidence

Everything in `data/` is synthetic and made for testing. No real mail, personal information, or organizational data is in this repository.

## Video Presentation

*Link to be added before submission.*

---

## Meeting the Assignment Requirements

The final project is worth 150 points across three deliverables. This section tracks how each one is being addressed.

### 1. Programming Project — 75 points

| Graded On | Points | Our Approach |
| --- | --- | --- |
| Functionality | 45 | The tool performs a real forensic task — email artifact extraction and analysis — and is built to handle malformed or corrupt files without crashing. |
| Code Quality | 15 | Work is split into separate modules by function rather than one large script, with comments explaining the reasoning behind each analysis step. |
| Repository & README | 15 | The repo follows the recommended folder structure. This README covers the overview, features, and setup instructions, and will link the video. |

### 2. Written Report — 45 points

The report will be added to `docs/` as a PDF.

| Graded On | Points | Our Approach |
| --- | --- | --- |
| Intro & Technical Details | 22 | Explains why phishing analysis is a relevant forensic problem, then walks through our language, library, and design choices and why we made them. |
| Results & Lessons Learned | 23 | Shows real sample output from the tool, then reflects on what worked, what didn't, and what we'd build differently. |

### 3. Video Presentation — 30 points

| Graded On | Points | Our Approach |
| --- | --- | --- |
| Content & Clarity | 15 | A 10–15 minute walkthrough of the purpose, design, and results, planned out ahead of time rather than improvised. |
| Tool Demo | 15 | A live run against a sample phishing email, rehearsed beforehand on a clean setup so the demo doesn't fail on a missing dependency. |

### Submission Checklist

- [x] Team members added as collaborators
- [x] Repository is public
- [x] `requirements.txt`, `.gitignore`, and `LICENSE` present
- [ ] `src/` contains working, commented code
- [ ] `data/` contains synthetic sample evidence
- [ ] `docs/final_report.pdf` uploaded
- [ ] Video uploaded and linked in this README
- [ ] Setup instructions tested on a clean machine

---

## AI Usage Disclosure

This README was drafted with the help of an AI language model. AI is also used inside the tool itself to summarize findings in plain language.

AI output in this project is advisory only. The actual forensic findings — headers, hashes, authentication results, extracted indicators — come from deterministic parsing, not from the model. The AI explains those findings; it doesn't produce them. The written report covers this in more detail, including the limits of using AI in a forensic context.

## License

MIT — see `LICENSE`.
