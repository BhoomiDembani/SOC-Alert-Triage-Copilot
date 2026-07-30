[README (2).md](https://github.com/user-attachments/files/30551795/README.2.md)
# SOC-Alert-Triage-Copilot#
🛡️ SOC Alert Triage Copilot

An AI-assisted triage system that automatically classifies, prioritizes, and explains security alerts — mapped to the MITRE ATT&CK framework — to help SOC (Security Operations Center) analysts cut through alert fatigue and focus on real threats.

```
Security Alerts (CSV/Log) → Flask API (Classification Engine) → MITRE ATT&CK Mapping → Analyst Dashboard
```

---

## 📌 Business Problem

SOC teams are flooded with thousands of security alerts every day — login attempts, port scans, malware signatures, unusual traffic — and the vast majority are false positives. Analysts spend most of their time manually sorting noise instead of investigating real threats, leading to a well-known industry problem called **alert fatigue**.

This project simulates a real-world SOC scenario: take raw, high-volume alert data and build an AI-assisted triage layer that classifies severity, maps each alert to a known attacker technique, and explains *why* — so analysts can act on the alerts that actually matter.

---

## 🎯 Objectives

- Classify raw security alerts by severity (Critical / High / Medium / Low)
- Map each alert to a known MITRE ATT&CK technique
- Generate a plain-English reason for every classification decision
- Present everything in a live, filterable analyst dashboard

---

## 🏗️ Architecture

```
┌─────────────────────────┐
│   Raw Security Alerts     │   sample_alerts.csv (synthetic SOC log data)
└────────────┬─────────────┘
             ▼
┌─────────────────────────┐
│    Flask API (Backend)    │   pandas — parses alerts, applies classification rules
└────────────┬─────────────┘
             ▼
┌─────────────────────────┐
│  Classification Engine    │   Severity scoring + MITRE ATT&CK technique mapping
└────────────┬─────────────┘
             ▼
┌─────────────────────────┐
│   /alerts JSON Endpoint    │   Each alert enriched with risk_label, mitre_id, reason
└────────────┬─────────────┘
             ▼
┌─────────────────────────┐
│   Analyst Dashboard        │   Live filters, search, threat-pulse summary bar
└─────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Backend / API | Python, Flask, Flask-CORS |
| Data Handling | Pandas |
| Threat Framework | MITRE ATT&CK |
| Frontend / Dashboard | HTML, CSS, JavaScript |
| Deployment | Render (backend), static hosting (dashboard) |
| Version Control | Git & GitHub |

---

## 📁 Project Structure

```
soc-alert-triage-copilot/
├── data/
│   └── sample_alerts.csv        # Synthetic sample security alerts
├── backend/
│   ├── app.py                   # Flask API + rule-based classifier
│   ├── requirements.txt
│   └── Procfile                 # For deployment (Render)
├── dashboard/
│   └── index.html               # Analyst console dashboard (frontend)
└── screenshots/
    └── (dashboard + API screenshots)
```

| Folder / File | Purpose |
|---|---|
| `data/sample_alerts.csv` | Synthetic SOC alert data used as the input source |
| `backend/app.py` | Flask API — reads alerts, classifies them, returns enriched JSON |
| `backend/requirements.txt` | Python dependencies for the backend |
| `backend/Procfile` | Start command used for deployment on Render |
| `dashboard/index.html` | Analyst console dashboard that visualizes triaged alerts |
| `screenshots/` | Dashboard and API screenshots referenced in this README |

---

## ⚙️ How to Run This Project

**1. Clone the repository**
```bash
git clone https://github.com/<your-username>/soc-alert-triage-copilot.git
cd soc-alert-triage-copilot/backend
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Run the Flask API**
```bash
python app.py
```

**4. Open the dashboard**
Open `dashboard/index.html` in your browser while the API is running — it will automatically fetch and display live triaged alerts.

---

## 📊 Screenshots

**Analyst Dashboard — Overview**

<img width="1366" height="681" alt="Screenshot 2026-07-30 134822" src="https://github.com/user-attachments/assets/fa9fe964-aba0-4fb0-8e48-234097c61275" />


**Analyst Dashboard — Alert List with MITRE Mapping**

<img width="1366" height="687" alt="Screenshot 2026-07-30 134910" src="https://github.com/user-attachments/assets/461c332a-62d4-49d6-8b7b-76eaac0a960a" />


**Backend API — Raw Classified Output**

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/b2b55c83-3748-4d42-b91b-0e3d749c86a9" />


---

## 💡 Key Design Decisions

- **Explainability over black-box scoring** — every alert includes a plain-English reason, not just a severity label, since analysts need to trust and verify AI decisions.
- **MITRE ATT&CK mapping** — grounds each alert in a real, industry-recognized attacker technique instead of a generic "risky/not risky" score.
- **Synthetic data by design** — real SOC log data is sensitive and can't be used in a student project; the dataset simulates realistic patterns (brute force, exfiltration, malware, reconnaissance) so the triage logic can be demonstrated safely.

---

## 🚀 Future Improvements

- Replace the rule-based classifier with an LLM-based reasoning layer for more nuanced triage decisions
- Add an analyst feedback loop so verdicts improve classification accuracy over time
- Connect to a live SIEM/log source instead of a static CSV
- Add authentication for a multi-analyst environment
- Deploy the dashboard live (e.g. Netlify/Vercel) alongside the Render-hosted backend

---

## 👩‍💻 Author

**Bhoomi Dembani**
BCA Student — Cybersecurity & Cloud Computing
 [LinkedIn](https://www.linkedin.com/in/bhoomi-dembani-87305b329) • [GitHub](https://github.com/bhumi1406d7-spec)
