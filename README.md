# Horizon: Student Wellness Check-in
 
A short, private check-in that turns everyday habits into a simple **wellness signal from 0 to 10**.
 
Students answer a few questions about a typical day (sleep, screen time, study, activity, stress). A machine learning model behind a FastAPI backend reads those patterns and returns a score. The front end shows it as a sun rising over a horizon: the higher the sun, the stronger the signal.
 
> **This is an informational reflection, not a clinical assessment.** It is not a diagnosis and does not replace professional care.
 
---
 
## Live demo
 
- **Live Demo:** `https://mental-health-score-vd45.onrender.com/`

> The API runs on a free Render instance, so the first request after a period of inactivity can take about 30–60 seconds while it wakes up.
 
---
 
## How it works
 
```
Browser (index.html)  ──POST /predict──▶  FastAPI backend  ──▶  ML model
        ▲                                                         │
        └────────────── predicted_mental_health_score ◀───────────┘
```
 
1. The user answers four short steps: about them, their phone use, their daily routine, and how stressed they have felt.
2. The page validates the answers and sends them as JSON to `POST /predict`.
3. The backend returns `predicted_mental_health_score`.
4. The front end shows the score and a short, supportive message.
### Score bands
 
| Score | Band | Meaning |
|-------|------|---------|
| Below 4 | Running low | Signs of strain. Small changes to sleep or screen time can help. |
| 4 to under 7 | Steady, with room to recover | A fairly even rhythm with room to rest. |
| 7 and above | Well supported | A resilient baseline. |
 
---
 
## Inputs
 
| Field | Type | Allowed values |
|-------|------|----------------|
| `age` | integer | 10 to 100 |
| `gender` | string | `Male`, `Female` |
| `country` | string | Any country |
| `academic_level` | string | `High School`, `Undergraduate`, `Graduate` |
| `most_used_platform` | string | Instagram, YouTube, WhatsApp, TikTok, Snapchat, Facebook, Twitter, LinkedIn, WeChat, LINE, KakaoTalk, VKontakte |
| `purpose_of_use` | string | `Networking`, `Education`, `Entertainment`, `News` |
| `avg_daily_usage_hours` | float | 0 to 24 |
| `daily_unlocks` | integer | 0 or more |
| `study_hours` | float | 0 to 24 |
| `physical_activity_hours` | float | 0 to 24 |
| `sleep_hours_per_night` | float | 0 to 24 |
| `stress_level` | string | `Low`, `Medium`, `High`, `Very High` |
 
## API
 
### `POST /predict`
 
**Request**
 
```json
{
  "age": 21,
  "gender": "Female",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Entertainment",
  "avg_daily_usage_hours": 5.5,
  "daily_unlocks": 80,
  "study_hours": 4,
  "physical_activity_hours": 1,
  "sleep_hours_per_night": 6.5,
  "stress_level": "Medium"
}
```
 
**Response**
 
```json
{ "predicted_mental_health_score": 6.42 }
```
 
Invalid input returns `422` with field-level details, which the UI reports back to the user.
 
---
 
The backend must allow cross-origin requests:
 
```python
from fastapi.middleware.cors import CORSMiddleware
 
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],   # restrict to your front-end URL in production
    allow_methods=["*"],
    allow_headers=["*"],
)
```
 
## Limitations
 
- The score is an estimate learned from patterns in data. It is not a measure of any individual's mental health.
- Results depend on the quality and coverage of the training data and may not generalize to every population.
- Answers are self-reported and describe a "typical day", so they can be imprecise.
- The tool should never be used to make clinical, academic or employment decisions.
## If you're struggling
 
If things feel heavy, please talk to someone you trust or a qualified professional. In an emergency, contact your local emergency number or a crisis line in your country.
 
 
Issues and pull requests are welcome. For larger changes, please open an issue first to discuss what you would like to change.
 
## License
 
Add a license of your choice, for example MIT.
