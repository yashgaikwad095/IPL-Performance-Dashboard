# IPL 2008–2024 Performance Dashboard & Analysis

Ball-by-ball dataset (225,954 rows, 950 matches, 2008–2024) analyzed end-to-end:
Power BI (Power Query + DAX) for the interactive dashboard, and a Python/pandas
notebook for deeper business-question analysis that goes beyond what the dashboard
visuals show on their own.

## Dataset

`ipl_data.csv` — ball-by-ball record with columns: match_id, season, venue, innings,
ball, batting_team, bowling_team, striker, non_striker, bowler, runs_off_bat, extras,
wides, noballs, byes, legbyes, penalty, wicket_type, player_dismissed.

## Dashboard (Power BI)

- 4 KPI cards, Total Runs by Year (trend), Total Runs by Batting Team, Top Run
  Scorers, Top Wicket Takers, Wicket Type breakdown, Matches by Venue
- Slicers: season/date range, Team Names
- Power Query cleaning: fixed type-detection errors on extras columns (wides,
  noballs, byes, legbyes, penalty), built a Teams dimension table

## Python Analysis (business insights beyond the dashboard)

**1. Match Win Analysis**
Computed each match's winner by comparing innings totals (team batting second wins
if their total exceeds the target). 948 of 950 matches had complete innings data
(2 excluded — likely no-result/abandoned games).

Caught and fixed a data-quality issue: "Rising Pune Supergiant" and "Rising Pune
Supergiants" were the same franchise recorded inconsistently across seasons —
merged before analysis.

*Finding:* Among established teams (150+ matches), Chennai Super Kings has the
highest win rate (58.2%) despite Mumbai Indians having more total wins (129 vs 121) —
CSK is more consistent. Gujarat Titans' 54.3% win rate (from only 46 matches, debut
championship season) is notable but flagged as a small-sample caveat rather than a
headline claim, same for other short-history franchises (<50 matches each).

**2. Batting Strike Rate Leaders**
Runs and balls faced per batter (min. 500 career runs to qualify, avoiding small-
sample outliers). League-wide average strike rate: 128.34.

*Finding:* Top strike-rate batters (AD Russell 177.77, Sunil Narine 162.70, Virender
Sehwag 155.44) run 20-38% above league average — mostly power-hitting
finisher-role players. AB de Villiers is the standout exception: highest total runs
(5,181) in the qualified list *and* a top-10 strike rate (151.89), showing rare
volume + speed combined.

**3. Bowling Economy Rate Leaders**
Runs conceded (runs off bat + wides + no-balls; byes/leg-byes excluded per cricket
scoring rules) per over bowled, minimum 600 legitimate balls (~100 overs) to qualify.

*Finding:* Spinners dominate the top-15 economical bowlers (Rashid Khan 6.38, Anil
Kumble 6.58, Sunil Narine 6.64, Muralitharan 6.70) — 8 of 15 are spin bowlers,
suggesting spin is more control-effective in T20 cricket. Dale Steyn (6.94) and
Malinga (7.14) are notable pace-bowling exceptions, especially given death overs
(16-20) run at a 9.38 runs/over league average vs. 7.38 in the powerplay (1-6) —
control bowling in that phase is rare and these two managed it.

## Data Quality Notes

- `wicket_type` blanks are empty strings, not true nulls — handled explicitly rather
  than relying on ISBLANK()
- Extras columns (wides/noballs/byes/legbyes/penalty) needed explicit fillna(0) —
  Power Query's auto-detection initially mis-typed them
- Team name inconsistencies handled: Delhi Daredevils → Delhi Capitals, Kings XI
  Punjab → Punjab Kings, Deccan Chargers → Sunrisers Hyderabad, Royal Challengers
  Bangalore → Royal Challengers Bengaluru, Rising Pune Supergiant(s) merged.
  Gujarat Lions and Gujarat Titans were kept separate — same city, different
  ownership/franchise, not the same team.
- 2 of 950 matches excluded from win analysis due to incomplete innings data
