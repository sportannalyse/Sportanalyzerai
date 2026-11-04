
import streamlit as st
import math

st.set_page_config(
    page_title="SPORT ANALYZER AI",
    page_icon="⚽",
    layout="wide"
)

# =========================
# HEADER
# =========================

st.title("🏆 SPORT ANALYZER AI")
st.subheader("Professional Sports Analysis Platform")

st.write(
    "Analyze matches using statistical models "
    "and team performance data."
)

st.divider()

sport = st.selectbox(
    "Choose your sport",
    ["Football", "Basketball", "Tennis"]
)

# =========================
# POISSON MODEL
# =========================

def poisson_probability(goals, expected_goals):
    return (
        math.exp(-expected_goals)
        * expected_goals ** goals
        / math.factorial(goals)
    )


def analyze_football(
    home_scored,
    home_conceded,
    away_scored,
    away_conceded
):
    # Estimated goals
    home_expected = (
        (home_scored + away_conceded) / 2
    ) * 1.10

    away_expected = (
        (away_scored + home_conceded) / 2
    )

    # Calculate score probabilities
    max_goals = 10

    home_probs = [
        poisson_probability(i, home_expected)
        for i in range(max_goals)
    ]

    away_probs = [
        poisson_probability(i, away_expected)
        for i in range(max_goals)
    ]

    matrix = []

    for h in range(max_goals):
        for a in range(max_goals):
            probability = home_probs[h] * away_probs[a]
            matrix.append((h, a, probability))

    total_probability = sum(x[2] for x in matrix)

    # Normalize probabilities
    matrix = [
        (h, a, p / total_probability)
        for h, a, p in matrix
    ]

    home_win = sum(
        p for h, a, p in matrix if h > a
    )

    draw = sum(
        p for h, a, p in matrix if h == a
    )

    away_win = sum(
        p for h, a, p in matrix if h < a
    )

    over_15 = sum(
        p for h, a, p in matrix if h + a > 1
    )

    over_25 = sum(
        p for h, a, p in matrix if h + a > 2
    )

    btts = sum(
        p for h, a, p in matrix if h > 0 and a > 0
    )

    top_scores = sorted(
        matrix,
        key=lambda x: x[2],
        reverse=True
    )[:5]

    return {
        "home_expected": home_expected,
        "away_expected": away_expected,
        "home_win": home_win,
        "draw": draw,
        "away_win": away_win,
        "over_15": over_15,
        "over_25": over_25,
        "btts": btts,
        "top_scores": top_scores
    }


# =========================
# FOOTBALL ANALYSIS
# =========================

if sport == "Football":

    st.header("⚽ Football Match Analyzer")

    st.info(
        "Enter each team's average goals scored "
        "and conceded per match over its last 5 matches."
    )

    col1, col2 = st.columns(2)

    with col1:
        st.subheader("🏠 Home Team")

        home_team = st.text_input(
            "Home team name",
            placeholder="Real Madrid"
        )

        home_scored = st.number_input(
            "Average goals scored",
            min_value=0.0,
            max_value=10.0,
            value=2.0,
            step=0.1
        )

        home_conceded = st.number_input(
            "Average goals conceded",
            min_value=0.0,
            max_value=10.0,
            value=1.0,
            step=0.1
        )

    with col2:
        st.subheader("✈️ Away Team")

        away_team = st.text_input(
            "Away team name",
            placeholder="Barcelona"
        )

        away_scored = st.number_input(
            "Average goals scored ",
            min_value=0.0,
            max_value=10.0,
            value=1.5,
            step=0.1
        )

        away_conceded = st.number_input(
            "Average goals conceded ",
            min_value=0.0,
            max_value=10.0,
            value=1.2,
            step=0.1
        )

    st.divider()

    if st.button(
        "🔍 ANALYZE MATCH",
        use_container_width=True
    ):

        if not home_team.strip() or not away_team.strip():
            st.warning("Please enter both team names.")

        else:
            result = analyze_football(
                home_scored,
                home_conceded,
                away_scored,
                away_conceded
            )

            st.success(
                f"Analysis: {home_team} vs {away_team}"
            )

            st.header("📊 Match Probabilities")

            c1, c2, c3 = st.columns(3)

            c1.metric(
                "Home Win",
                f"{result['home_win'] * 100:.1f}%"
            )

            c2.metric(
                "Draw",
                f"{result['draw'] * 100:.1f}%"
            )

            c3.metric(
                "Away Win",
                f"{result['away_win'] * 100:.1f}%"
            )

            st.divider()

            st.subheader("⚽ Goal Predictions")

            c4, c5, c6 = st.columns(3)

            c4.metric(
                "Over 1.5 Goals",
                f"{result['over_15'] * 100:.1f}%"
            )

            c5.metric(
                "Over 2.5 Goals",
                f"{result['over_25'] * 100:.1f}%"
            )

            c6.metric(
                "Both Teams To Score",
                f"{result['btts'] * 100:.1f}%"
            )

            st.divider()

            st.subheader("🎯 Expected Goals")

            st.write(
                f"{home_team}: "
                f"{result['home_expected']:.2f} goals"
            )

            st.write(
                f"{away_team}: "
                f"{result['away_expected']:.2f} goals"
            )

            st.divider()

            st.subheader("🏆 Most Probable Scores")

            for h, a, probability in result["top_scores"]:
                st.write(
                    f"**{h} - {a}**"
                    f"  |  {probability * 100:.2f}%"
                )

            st.divider()

            st.caption(
                "Statistical estimate based on the data entered. "
                "Not a guaranteed result or betting advice."
            )

# =========================
# OTHER SPORTS
# =========================

elif sport == "Basketball":
    st.header("🏀 Basketball Analyzer")
    st.info(
        "Basketball analysis module is planned "
        "for the next development stage."
    )

elif sport == "Tennis":
    st.header("🎾 Tennis Analyzer")
    st.info(
        "Tennis analysis module is planned "
        "for the next development stage."
    )

# =========================
# FOOTER
# =========================

st.divider()

st.caption(
    "SPORT ANALYZER AI | Version 2.0 | 2026"
)
