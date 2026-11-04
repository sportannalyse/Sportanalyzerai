# Sportanalyzeraiimport streamlit as st

st.set_page_config(
    page_title="SPORT ANALYZER AI",
    page_icon="⚽",
    layout="wide"
)

st.title("🏆 SPORT ANALYZER AI")
st.subheader("Your Professional Sports Analysis Platform")

st.write("Welcome to SPORT ANALYZER AI!")

st.info(
    "Our platform will analyze football, "
    "basketball and tennis matches."
)

sport = st.selectbox(
    "Choose your sport",
    ["Football", "Basketball", "Tennis"]
)

st.write("### 📊 Match Analysis Center")

team1 = st.text_input("First team or player")
team2 = st.text_input("Second team or player")

if st.button("Analyze Match"):
    if team1 and team2:
        st.success("Match registered successfully!")
        st.write("Sport:", sport)
        st.write("Match:", team1, "vs", team2)
        st.info("Statistical analysis engine will be connected in the next stage.")
    else:
        st.warning("Please enter both teams or players.")

st.divider()
st.caption("SPORT ANALYZER AI | Version 1.0")
st.caption("Created in 2026")
Ai sports Analysis platform
import streamlit as st

st.set_page_config(
    page_title="SPORT ANALYZER AI",
    page_icon="⚽",
    layout="wide"
)

st.title("🏆 SPORT ANALYZER AI")
st.subheader("Your Professional Sports Analysis Platform")

st.write("Welcome to SPORT ANALYZER AI!")

st.info(
    "Our platform will analyze football, "
    "basketball and tennis matches."
)

sport = st.selectbox(
    "Choose your sport",
    ["Football", "Basketball", "Tennis"]
)

st.write("### 📊 Match Analysis Center")

team1 = st.text_input("First team or player")
team2 = st.text_input("Second team or player")

if st.button("Analyze Match"):
    if team1 and team2:
        st.success("Match registered successfully!")
        st.write("Sport:", sport)
        st.write("Match:", team1, "vs", team2)
        st.info("Statistical analysis engine will be connected in the next stage.")
    else:
        st.warning("Please enter both teams or players.")

st.divider()
st.caption("SPORT ANALYZER AI | Version 1.0")
st.caption("Created in 2026")
