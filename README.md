# Modeling Early-Session Engagement Dynamics Using Spotify Listening History

## Overview

This project analyzes ~240,000 Spotify listening events (~11,000 sessions) to investigate whether early-session behavior predicts session depth.

The goal was to determine whether skip behavior reflects dissatisfaction or engagement calibration, and whether novelty/recency signals influence session survival.

---

## Data

- Spotify Extended Streaming History export  
- ~240K listening events  
- ~11K sessions constructed using a 30-minute inactivity threshold  

> Note: Raw Spotify data is not included in this repository.

---

## Methodology

### 1. Data Processing
Load and clean Spotify Extended Streaming History logs
- Load all `.json` streaming history files
- Filter non-music events and unnecessary metadata
- Parse timestamps and order listening events chronologically
- Construct a clean event-level dataset of listening behavior

Output: **~240K listening events**

### 2. Listening Session Construction
Define listening sessions using behavioral inactivity thresholds
- Define a new session when inactivity exceeds 30 minutes
- Remove sessions shorter than 2 tracks

Output: **11K listening sessions** 

### 3. Fatigue Feature Engineering
Analyze how skip behavior evolves within sessions

Features: 
- `active_skip`: forward button pressed mid-play
- `position_id`: track position within a session
- `skip_by_position`: probability of skipping by track position

Purpose: capture fatigue effects as sessions progress

### 4. Novelty Feature Engineering
Measure familiarity and novelty using listening history. 

Features: 
- `lifetime_track_plays`: total historical plays of a track
- `days_since_last_track_play`: recency of prior exposure
- `recency_bin`: categorical recency buckets
  (≤1d, 1–7d, 7–30d, 30–90d, 90–365d, 1y+)

Analysis examines how novelty and familiarity affect skip probability

### 5. Session Level Feature Engineering
Aggregate event-level data to analyze session dynamics

Session attributes:
- `session_len`: number of tracks in session
- `session_duration_minutes`: total session duration
- `skip_rate`: average skip frequency
- `first_listen_rate`: proportion of first-time listens
- `mean_recency_days`: average track recency

Define engagement outcome: 
- `long_session`: indicator for sessions > 10 tracks

Output: **session_level dataset** 

### 6. Early-Session Feature Engineering (First 3 Tracks)
Measure early-session user behavior to capture calibration dynamics

Early-session features:
- `early_intervention_rate`: frequency of manual overrides
  (`fwdbtn`, `backbtn`, `clickrow`)
- `early_first_listen_rate`: proportion of first-time tracks
- `early_mean_recency`: average days since last listen

These features capture how users actively tune their listening environment at session start

### 7. Predictive Modeling
Predict session depth using early-session behavioral signals
- Target variable: `Long session` (>10 tracks)
- Model: Logistic regression classifier
- Evaluation metric: **AUC**

Goal: estimate whether early engagement behavior predicts deeper listening sessions

---

## Key Findings

- Sessions with high early intervention (2–3 overrides in first 3 tracks) were ~4–5 percentage points more likely to become long sessions.
- Early override behavior was more predictive of session survival than novelty or recency.
- Skip behavior appears to function as engagement calibration rather than dissatisfaction.
- Novelty (first-time listens) showed weak or slightly negative association with session depth.

**Model AUC:** ~0.54 (above baseline, modest but interpretable)

---

## Product Implications

- Early-session override behavior may signal active engagement rather than churn risk.
- Engagement tuning in the first few interactions is critical for session depth.
- Skip metrics should not be naively interpreted as dissatisfaction signals.

---

## How to Run

1. Export Spotify Extended Streaming History from your Spotify account.
2. Place JSON files locally (not included in repo).
3. Install dependencies:
    ```bash
    pip install -r requirements.txt
4. Run the notebook