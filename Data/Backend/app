"""
SOC Alert Triage Copilot - Day 1
Basic Flask API that reads fake alerts and classifies them
using simple rule-based logic (no AI yet - that comes later).
"""

from flask import Flask, jsonify
from flask_cors import CORS
import pandas as pd
import os
import re

app = Flask(__name__)
CORS(app)  # allows the dashboard (opened as a separate file/origin) to fetch this API

# Path to the CSV (goes up one folder, into data/)
CSV_PATH = os.path.join(os.path.dirname(__file__), "..", "data", "sample_alerts.csv")


def classify_alert(alert):
    """
    Very simple rule-based classifier.
    Looks at alert_type and description text to decide risk level.
    Also does a basic MITRE ATT&CK technique guess.
    Returns: (risk_label, mitre_id, reason)
    """
    alert_type = alert["alert_type"].lower()
    description = alert["description"].lower()

    # Try to find "x<number>" pattern like "x5", "x20" for repeated failures
    match = re.search(r"x(\d+)", description)
    fail_count = int(match.group(1)) if match else 0

    # --- Rule-based decisions ---
    if "brute force" in alert_type or fail_count >= 10:
        return (
            "Critical",
            "T1110 - Brute Force",
            f"High number of repeated failed logins ({fail_count}) from a single IP suggests a brute force attempt."
        )

    if "login failure" in alert_type and fail_count >= 5:
        return (
            "High",
            "T1110 - Brute Force",
            f"{fail_count} failed logins in a short window is suspicious and matches brute force behavior."
        )

    if "data exfiltration" in alert_type:
        return (
            "Critical",
            "T1041 - Exfiltration Over C2 Channel",
            "Large or repeated outbound transfers to an unknown destination indicate possible data theft."
        )

    if "malware" in alert_type:
        return (
            "Critical",
            "T1204 - User Execution",
            "A known malicious file or signature was detected, indicating a likely malware delivery attempt."
        )

    if "port scan" in alert_type:
        return (
            "Medium",
            "T1046 - Network Service Discovery",
            "Scanning multiple ports is a common reconnaissance step before an attack."
        )

    if "login failure" in alert_type and fail_count < 5:
        return (
            "Low",
            "N/A",
            "A small number of failed logins is common (e.g. a typo) and not alone a strong indicator of attack."
        )

    # Default: routine/benign activity
    return (
        "Low",
        "N/A",
        "This matches normal, expected user or system behavior."
    )


@app.route("/alerts", methods=["GET"])
def get_alerts():
    df = pd.read_csv(CSV_PATH)
    alerts = df.to_dict(orient="records")

    for alert in alerts:
        risk_label, mitre_id, reason = classify_alert(alert)
        alert["risk_label"] = risk_label
        alert["mitre_id"] = mitre_id
        alert["reason"] = reason

    return jsonify(alerts)


@app.route("/", methods=["GET"])
def home():
    return {
        "message": "SOC Alert Triage Copilot API is running.",
        "try_this": "/alerts"
    }


if __name__ == "__main__":
    app.run(debug=True, port=5000)
