# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## What this is

A single-file static web app (`index.html`) for running a World Cup 2026 sweepstakes draw.
Name: **The Shitetalk World Cup 2026 Sweepstakes**

## Deployment

The app is hosted on Google Cloud Storage as a public static site:

```bash
# Deploy after any change
gcloud storage cp index.html gs://shitetalk-sweepstakes/index.html --content-type=text/html

# Live URL
https://storage.googleapis.com/shitetalk-sweepstakes/index.html
```

GCS bucket is in project `adams-world`, region `europe-west1`.

## Architecture

Everything lives in `index.html` — no build step, no dependencies, no CDN libs.

**Key sections of the script:**
- `TEAMS` — array of 48 teams, each with `name`, `flag` (emoji), and `rank` (1–48). Edit here to correct rankings or swap teams.
- `setCount(n)` / `renderNameFields(n)` — manages the entrant count UI and name input grid
- `shuffle()` — Fisher-Yates in-place shuffle
- `drawBtn click handler` — slices top `n*2` teams, shuffles, assigns 2 per entrant
- `renderResults()` — builds result cards with staggered CSS reveal animation

**Draw logic:**
- 24 entrants → all 48 teams used
- < 24 entrants → top `n * 2` teams used, lowest-ranked teams are excluded
- Teams are shuffled after slicing, so draw order is random within the active pool
