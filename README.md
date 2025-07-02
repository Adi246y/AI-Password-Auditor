# AI-Password-Auditor
ai-password-checker/
├── app.py
├── password_checker.py
├── model/
│   └── pattern_model.pkl
├── common_passwords.txt
├── requirements.txt
└── README.md
import re
import difflib

def load_common_passwords(filepath="common_passwords.txt"):
    with open(filepath, "r") as f:
        return set(line.strip() for line in f)

def password_strength(password, common_pw_set):
    score = 100
    reasons = []

    if len(password) < 8:
        score -= 25
        reasons.append("Password too short (<8 characters)")

    if not re.search(r"[A-Z]", password):
        score -= 15
        reasons.append("No uppercase letters")

    if not re.search(r"[0-9]", password):
        score -= 15
        reasons.append("No digits")

    if not re.search(r"[^a-zA-Z0-9]", password):
        score -= 10
        reasons.append("No special characters")

    if password.lower() in common_pw_set:
        score -= 40
        reasons.append("Password found in common passwords list")

    return max(score, 0), reasons
    import streamlit as st
from password_checker import load_common_passwords, password_strength

st.set_page_config(page_title="AI Password Auditor", layout="centered")

st.title("🔐 AI Password Strength Checker")

pw = st.text_input("Enter your password", type="password")

if pw:
    common_pw_set = load_common_passwords()
    score, reasons = password_strength(pw, common_pw_set)

    st.markdown(f"### Strength Score: `{score}/100`")

    if reasons:
        st.warning("Weaknesses detected:")
        for r in reasons:
            st.markdown(f"- {r}")
    else:
        st.success("Strong password! ✅")
        streamlit
