# PS 07 — Personalized Fitness Plan Using Activity and Recovery Data

A README distilling the raw notes into what's essential to build, what's optional/nice-to-have, and what can be dropped or simplified for a hackathon build.

---

## 1. Core Architecture (Keep — this is your spine)

```
User Profile + Goals
        ↓
Activity Data (steps, HR, workout, calories)
Recovery Data (sleep, HRV, resting HR, fatigue, soreness)
Historical Data (past workload & recovery)
        ↓
Personal Baseline Engine
        ↓
Recovery + Training Load Analysis
        ↓
Adaptive Recommendation Engine
        ↓
Today's Workout / Rest / Recovery
        ↓
User Feedback
        ↓
Plan continuously adapts (loop back to top)
```

**Why this matters:** the feedback loop is the single most important idea in the whole notes dump. A fixed 30-day plan that never updates fails the problem statement outright — "adjust ... based on longitudinal trends" is explicitly required. Everything else in this README exists to serve this loop.

---

## 2. What to actually build (essential, in priority order)

### 2.1 Personal Baseline Engine — **Keep, build first**

Instead of generic thresholds ("7 hrs sleep is good"), compute each user's own rolling average for sleep, resting HR, training load, and soreness, then compare _today_ against _their_ normal.

- Simple to implement: rolling 14–28 day average per metric.
- This is your strongest differentiator from generic fitness apps — lead with it in your pitch.

### 2.2 Recovery Score (0–100) — **Keep, but simplify the weights**

A weighted composite score. Use this exact structure, it's good:

| Factor               | Weight |
| -------------------- | ------ |
| Sleep                | 25%    |
| HRV / resting HR     | 20%    |
| Fatigue              | 15%    |
| Soreness             | 15%    |
| Recent training load | 15%    |
| Baseline deviation   | 10%    |

Bands: 80–100 High readiness · 60–79 Moderate · 40–59 Low · 0–39 Recovery required.

**Note for your submission:** explicitly state these weights/thresholds are _configurable starting parameters_, not validated medical values. Judges will ask about this — get ahead of it.

### 2.3 Training Load Engine — **Keep, but start simple**

Don't overbuild this initially. MVP formula:

```
Training Load = Workout Duration × Perceived Intensity (RPE)
```

Example: 60 min × 8/10 RPE = 480

Track rolling windows: today, 7-day, 28-day. This is what lets you compute ACWR-style spike detection (sudden jumps, excessive consecutive hard days, prolonged low activity).

_Optional upgrade if time permits:_ incorporate heart-rate-based load instead of just RPE.

### 2.4 Adaptive Recommendation Engine — **Keep, this is "the heart" — build it as a rule table, not ML**

Output isn't just a workout type — it's intensity + type + duration + volume, all scaled by recovery score and load trend.

Bad output: _"Today: Chest workout."_
Good output: _"Moderate upper-body workout, 40 minutes, 15% reduced volume — recovery is below your baseline and 7-day load is elevated."_

A rule-based lookup table (recovery band × load trend × goal → session spec) is enough for SIH. Don't spend hackathon time on ML here — a defensible, explainable rule engine scores better than a black box.

### 2.5 Explainable Recommendations — **Keep, high priority, low effort**

Every recommendation ships with a one-line "Why?" built directly from the numbers that drove it:

> _"Recovery score is 58/100. Sleep was 2.1h below your baseline and training load has risen for 3 consecutive days — intensity reduced today."_

This is cheap to implement (just string-template your existing variables) and disproportionately impresses judges because it proves the system isn't random.

### 2.6 Continuous Adaptation Loop — **Keep — this is what judges will ask to see demoed**

User logs post-workout feedback (RPE, soreness, energy) → feeds back into recovery score → next day's plan shifts accordingly. Prepare a 5-day mock sequence for your demo (recovery swinging 82 → 71 → 48 → 35 → 74) so you can visually show the plan reacting in real time.

### 2.7 Dual Data Input (manual + wearable) — **Keep — explicitly required by the PS**

- Manual: sleep, fatigue, soreness, workout, intensity, weight, mood.
- Wearable/API (Google Fit / Health Connect, or simulated): steps, HR, sleep, calories, distance.
- **Manual input is the fallback when wearable data is missing** — say this explicitly in your architecture slide, it shows you thought about real-world reliability.

### 2.8 Longitudinal Trend Dashboard — **Keep — explicitly required by the PS outcome**

Minimum viable charts: Recovery trend, Training load trend, Sleep trend (line charts, 7–14 day window). The system should be able to surface a plain-language insight like:

> _"Recovery has declined for 4 consecutive days while training load increased."_

That sentence is almost a direct quote of the PS outcome requirement — make sure your demo produces something like it.

---

## 3. Nice-to-have (only if time remains)

- **Weekly auto-generated schedule view** (Mon–Sun with color-coded intensity) — a good visual for your PPT, but not core logic. Build the underlying day-by-day recommendation first; this is just a UI wrapper around it.
- **HRV integration** — good to mention as a roadmap item, don't block your MVP on sourcing real HRV data.
- **ML-based load prediction** — mention as future work/stretch goal only. A rule-based system is more explainable and easier to defend live.

---

## 4. Cut or compress in your final submission

- Don't present the "Recovery Score formula" as a literal sum (`Sleep + HR/HRV + Fatigue...`) — it's a _weighted_ score, present it as the percentage table instead (already reflected above). The plain-sum notation in the raw notes is misleading.
- Don't over-elaborate the weekly schedule emojis (🔴🟡🟢⚪) in your actual pitch deck — fine for internal notes, but use a proper color-coded UI mockup instead for judges.
- Don't present training load, recovery score, and baseline deviation as three separate unrelated features in your pitch — frame them as one pipeline (baseline → recovery score → load trend → recommendation) so judges see the system thinking, not a feature list.

---

## 5. One-line pitch (for your PPT title slide)

> An adaptive fitness app that learns _your_ personal baseline — not generic norms — and continuously re-tunes today's workout intensity, type, and volume based on your recovery and training load trends, with every recommendation explained in plain language.

## techninacl approach

1. Technologies to be Used
   A. Frontend
   React.js

Use React.js to build the web/mobile-friendly user interface.

The frontend will provide:

User registration/login
Fitness goal selection
Activity input
Recovery input
Dashboard
Today's workout
Workout history
Recovery statistics
Training-load graphs
Weekly fitness plan
Recommendation explanation
Progress analytics
Tailwind CSS

Use Tailwind CSS for:

Responsive UI
Dashboard design
Cards
Charts layout
Forms
Mobile responsiveness
Recharts

Use Recharts for visualizing:

Recovery trends
Training load
Sleep trends
Weekly activity
Workout consistency
Fitness progress
