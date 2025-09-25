# AI Daily Prep Agent

An automation agent that:
- Reviews calendar events and prepares meeting context
- Checks Gmail for important emails
- Reviews LinkedIn invites & messages
- Applies to targeted AI/ML/CV/Robotics jobs
- Connects with relevant professionals and sends tailored messages
- Creates & schedules LinkedIn posts
- Sends daily email summaries (Agenda, Insights, LinkedIn Review, Activity Summary, Post Summary)

> Built to focus on Machine Learning, AI Agents, Computer Vision, and Robotics.

## Features
- **Email digest**: summarize important threads and action items
- **LinkedIn networking**: auto-screen invites, propose accept/decline, draft messages
- **Job search & quick-apply**: configurable filters, tracked applications
- **Content posting**: template-driven post generator with hashtags
- **Privacy-first config**: credentials via `.env` and `config.yaml`

## Architecture
```
src/
  main.py                # entrypoint (CLI or cron)
  orchestrator.py        # schedules and coordinates tasks
  email.py               # Gmail retrieval + summarization
  linkedin.py            # invitations, messaging, posting
  jobs.py                # job search + application logic
  templates.py           # template loader & renderer
  utils.py               # logging, time utils, persistence
templates/
  connection_message.txt # connection DM template
  linkedin_post.md       # LinkedIn post template
config.example.yaml      # user preferences & targeting
.env.example             # credentials (do not commit real keys)
```
- **Stateless runs** are supported; persisted data (e.g., last run cursors) stored under `~/.ai_daily_prep/` by default.

## Tech Stack
- Python 3.10+
- Optional: OpenAI/LLM API for summarization and message drafting
- (Integrations) Gmail API, LinkedIn automation (headful browser or API where available)

## Quick Start
1. **Clone** this repo and create a virtual environment:
   ```bash
   git clone <your-repo-url> ai-daily-prep-agent
   cd ai-daily-prep-agent
   python -m venv .venv && source .venv/bin/activate
   pip install -r requirements.txt
   ```
2. **Configure credentials**:
   - Copy `.env.example` to `.env` and fill in values.
   - Copy `config.example.yaml` to `config.yaml` and adjust preferences.
3. **Run locally**:
   ```bash
   python -m src.main --daily
   ```
4. **Schedule (cron)**:
   ```bash
   # Run at minute 5 every hour
   5 * * * * /path/to/python /path/to/repo/src/main.py --hourly >> ~/ai_agent.log 2>&1
   ```

## Configuration
`config.yaml` controls targeting and behavior:
```yaml
profile:
  full_name: "Panagiota Grosdouli"
  interests: ["Machine Learning", "AI Agents", "Computer Vision", "Robotics"]

email:
  summaries:
    agenda: true
    insights: true
    linkedin_review: true
    activity_summary: true
    post_summary: true

linkedin:
  connect_strategy:
    target_roles: ["ML Engineer", "AI Researcher", "Computer Vision Engineer", "Robotics Engineer"]
    target_companies: ["OpenAI", "DeepMind", "NVIDIA", "Boston Dynamics", "Waymo"]
    include_recruiters: true
    max_requests_per_day: 15
  messaging:
    follow_up_days: 7
    rate_limit_per_hour: 10

jobs:
  search:
    keywords: ["Machine Learning", "Computer Vision", "Robotics", "AI Agent"]
    locations: ["Remote", "Athens, GR", "EU"]
    experience_levels: ["Entry", "Junior", "Mid"]
  apply:
    auto_apply_quick: true
    review_queue: true

posting:
  enable: true
  cadence: "2/week"     # or "daily", "weekly"
  topics:
    - "Practical ML tips"
    - "AI Agents in production"
    - "Computer Vision pipelines"
    - "Robotics & perception"
```

## Templates
- `templates/connection_message.txt` uses lightweight placeholders:
  ```text
  Hi {{name}}, I noticed your work on {{topic}} at {{company}}.
  I'm focusing on {{interests}} and recently built an AI agent that supports daily workflows.
  Would love to connect and exchange insights!
  ```
- `templates/linkedin_post.md` example:
  ```markdown
  🚀 Just built my own AI Agent!
  
  Focus areas: **Machine Learning, AI Agents, Computer Vision, Robotics**.
  It automates networking, job applications, and knowledge sharing—
  exploring how AI systems can learn, adapt, and support daily workflows.
  
  What would you build next with AI agents?
  
  #AI #MachineLearning #AIagents #ComputerVision #Robotics #DeepLearning
  ```

## Development
- Code style: `ruff` + `black`
- Testing: `pytest`

## Roadmap
- Headless browser fallback
- Smarter deduping/throttling for invites + follow-ups
- In-depth analytics for outreach & application funnel

## Security & Ethics
- Never send messages or apply without explicit consent where required.
- Respect platform ToS and legal constraints.
- Store tokens securely; prefer OAuth where available.

## License
[MIT](./LICENSE)
