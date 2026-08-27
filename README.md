# IT360-Group-Project

**Phishing Email Forensics Analyzer**

Tyler Lyons, Sam Emerson, and Vadim Korypaev — IT 360 Final Project

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

The core analyzer requires **Python 3.8+** and no third-party packages. The optional dependencies enable the AI summary and Outlook `.msg` support.

```bash
git clone https://github.com/<user>/IT360-Group-Project.git
cd IT360-Group-Project

# Optional but recommended
pip install -r requirements.txt

# Generate the synthetic test samples
python data/make_samples.py

# Verify everything works
python src/test_analyzer.py
```

To enable the AI summary, set your API key:

```bash
export ANTHROPIC_API_KEY="your-key-here"      # macOS / Linux
setx ANTHROPIC_API_KEY "your-key-here"        # Windows
```

Without a key the tool runs normally and skips only the summary section.

## Usage

```bash
# Analyze one message, print to terminal
python src/main.py --input data/02_spoofed_sender.eml

# Analyze every sample and write all report formats
python src/main.py --input data/ --output reports/ --format all

# Skip the AI summary
python src/main.py --input data/03_lookalike_domain.eml --no-ai
```

| Flag | Purpose |
| --- | --- |
| `-i`, `--input` | Email file or directory of them (required) |
| `-o`, `--output` | Report output directory (default: `reports/`) |
| `-f`, `--format` | `console`, `json`, `csv`, `html`, or `all` |
| `--no-ai` | Skip the AI summary |
| `--model` | Override the model used for the summary |
| `--audit-log` | Custom audit log path |
| `-q`, `--quiet` | Suppress console output |
| `--no-color` | Disable ANSI color |

**Exit codes:** `0` nothing flagged, `1` at least one message flagged SUSPICIOUS or MALICIOUS, `2` fatal error. This lets the tool be used in scripts and scheduled jobs.

## Module Layout

| File | Responsibility |
| --- | --- |
| `src/main.py` | CLI, orchestration, batch processing |
| `src/evidence.py` | Hashing, integrity verification, audit log |
| `src/email_parser.py` | `.eml` / `.msg` parsing into one normalized structure |
| `src/header_analysis.py` | Received chain, SPF/DKIM/DMARC, spoofing, lookalike domains |
| `src/url_analysis.py` | URL extraction, unwrapping, defanging, suspicion checks |
| `src/attachment_analysis.py` | Hashing, magic-byte verification, dangerous extensions |
| `src/scoring.py` | Deterministic rule-based risk score |
| `src/ai_summary.py` | Anthropic API integration for plain-language explanation |
| `src/report.py` | Console, JSON, CSV, and HTML output |
| `src/test_analyzer.py` | Test suite (58 tests, standard library only) |
| `data/make_samples.py` | Synthetic sample generator |

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

- [x] Repository is public
- [x] `src/` contains working, commented code
- [x] `data/` contains synthetic sample evidence
- [x] `requirements.txt`, `.gitignore`, and `LICENSE` present
- [ ] `docs/final_report.pdf` uploaded
- [ ] Video uploaded and linked in this README
- [ ] Setup instructions tested on a clean machine

---

## AI Usage Disclosure

**This README and the source files in `src/` and `data/` were generated by Claude Opus 5 (Anthropic)** at our direction, then reviewed and adapted by us. Each source file carries an "AI GENERATION NOTICE" header stating this.

AI is also used *inside* the tool, in `src/ai_summary.py`, to write a plain-language explanation of the results.

**The boundary we drew, and why it matters:**

The language model receives only the structured findings that the deterministic modules already produced. It never sees the raw email. It is not asked whether a message is malicious — `scoring.py` answers that by rule, before the AI module runs.

Three reasons:

1. **Reproducibility.** Findings and score are identical on every run. If a model made the determination, two runs on the same evidence could disagree, which is fatal to an evidentiary process.
2. **Explainability.** Every finding traces back to a named rule in the source. "The model thought it looked suspicious" is not defensible in an incident review.
3. **Prompt injection resistance.** Phishing bodies are attacker-authored text. A model that both reads that text and influences the verdict can be attacked directly — the attacker simply writes instructions into the message. Passing only our own structured findings closes that path.

In short: **rules decide, AI explains.** The written report discusses this trade-off and its limitations in detail.

## License

MIT — see `LICENSE`.
