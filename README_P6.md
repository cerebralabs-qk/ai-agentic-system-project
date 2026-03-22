# P6 — Agentic AI Systems
## CEREBRA-DX Screening Coordinator: ReAct Agent for Early Dementia Triage

**Udacity Machine Learning Engineer Nanodegree**  
**Author:** Qeis Kamran | GitHub: [cerebralabs-qk](https://github.com/cerebralabs-qk)

---

## Project Summary

This project implements **CEREBRA-DX Screening Coordinator**, a single-agent clinical decision support system using the **ReAct (Reasoning + Acting)** pattern (Yao et al., 2023) for early dementia pre-screening triage. The agent integrates multi-modal patient data — cognitive scores (MMSE, CDR), demographic risk factors, and biomarker proxies — into a structured diagnostic recommendation pathway.

The system operationalises the pre-screening logic of the CEREBRA-DX diagnostic platform, a multimodal early dementia detection system targeting DiGA certification and EU market entry (Kamran & Baretto, 2025).

---

## Agent Architecture

| Component | Specification |
|---|---|
| Pattern | ReAct — Thought → Action → Observation (Yao et al., 2023) |
| Type | Single-agent with tool use, session memory, audit logging |
| Tools | 4: risk scoring, NIA-AA biomarker lookup, DDx engine, report generator |
| Short-term memory | SessionMemory dataclass — active patient state, ReAct trace |
| Long-term memory | cerebra_audit_log.jsonl — persistent JSONL audit log |
| Safeguards | 5: input validation, hard escalation (CDR >= 1.5), confidence gate, iteration cap, mandatory disclaimer |

---

## Submission Files

| File | Description |
|---|---|
| `agentic_system.ipynb` | Main notebook — runs top-to-bottom, no external API required |
| `Agentic_AI_System_Design_Report.pdf` | Full system design and ethics report (13 pages, 16 references) |
| `requirements.txt` | Reproducibility file |

---

## Quick Start

```bash
# 1. Clone
git clone https://github.com/cerebralabs-qk/ai-agentic-system-project.git
cd ai-agentic-system-project

# 2. Install
pip install -r requirements.txt

# 3. Run
jupyter notebook agentic_system.ipynb
# Kernel → Restart & Run All
```

> No API keys required. All patient data is synthetic and programmatically generated. Runs fully offline.

---

## Key Behaviors Demonstrated

- **PT-001** (CDR=0.0, MMSE=28) — Risk=0.127 (LOW) — Normal Cognitive Aging — Annual monitoring
- **PT-002** (CDR=0.5, MMSE=24, ApoE4+) — Risk=0.455 (BORDERLINE) — AD pre-clinical — Full neuropsych battery
- **PT-003** (CDR=1.0, MMSE=19, A+T+) — Risk=0.764 (HIGH) — AD p=0.89 — Urgent referral + lecanemab eligibility
- **PT-004** (CDR=2.0) — **Hard escalation triggered** — 0 ReAct iterations — Immediate clinical review
- **PT-005** (CDR=0.5, A-T+) — MCI amnestic + FTD differential — Edge case ambiguity documented
- **PT-ERR** (invalid MMSE/CDR) — **Input validation abort** — 3 errors identified, session terminated

---

## Clinical Context

This agent directly addresses the dementia diagnostic gap — the average 2-3 year delay between symptom onset and formal diagnosis. By triaging patients before specialist appointments, the system directs limited clinical resources toward high-risk individuals, supporting the lecanemab/donanemab treatment window where disease-modifying therapy is most effective (van Dyck et al., 2023).

---

## Safeguard Architecture

```
INPUT: PatientRecord (14 fields)
  | Safeguard 1: Input Validation (schema check all fields)
  | Safeguard 2: Hard Escalation Gate (CDR >= 1.5 → ESCALATE)
  | ReAct Loop (max 10 iterations)
      Thought → Action → Observation
      Safeguard 3: Confidence Gate (score < 0.40 → flag)
      Safeguard 4: Iteration Cap (max 10 → terminate)
  | Audit Log (JSONL, append-only)
OUTPUT: Structured Report + Safeguard 5: Mandatory Disclaimer
```

---

## References

- Yao, S. et al. (2023). ReAct: Synergizing reasoning and acting in language models. *ICLR 2023*
- Kamran, Q., & Baretto, P. (2025). Fathoming Ashby's Law of Requisite Variety. *Springer Nature* [TR-1]
- Marcus, D.S. et al. (2007). OASIS. *Journal of Cognitive Neuroscience, 19*(9), 1498-1507
- van Dyck, C.H. et al. (2023). Lecanemab in early Alzheimer's disease. *NEJM, 388*(1), 9-21
- Full reference list: see `Agentic_AI_System_Design_Report.pdf`
