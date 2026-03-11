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
- Load in all .json files
- Drop all non-music data and unnecessary features
- Parse timestamps and order listening events chronologically
- Construct a clean event-level dataset of listening behavior

### 2. Listening Session Construction
- Defined session boundaries using 30-minute inactivity gaps.
- Filter out sessions less than 2 tracks long
- Output: **11K listening sessions** 

### 3. Fatigue Feature Engineering
- **active_skip** (if user clicked forward button mid-play)
- **position_id** 
- **skip_by_position** (probability of skipping given position in a session)

### 4. Novelty Feature Engineering
- Analysis of skip rate by absolute and relative novelty metrics
- **lifetime_track_plays** (Lifetime plays of a track)
- **days_since_last_track_play** (Days since last time track was played)

### 5. Session Level Table
- Aggregate session level attributes:
  - session_len (number of tracks in session)
  - skip_rate (average amount of skips in session)
  - 

### 5. Early-Session Feature Engineering (First 3 Tracks)
- **Early intervention rate**  
  (`fwdbtn`, `backbtn`, `clickrow`)
- **Early first-listen rate**
- **Early recency** (days since prior play)

### 4. Predictive Modeling
- Target: Long session (>10 tracks)
- Logistic regression model
- Evaluation via AUC

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