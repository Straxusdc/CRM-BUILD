# crm-builder-academy: Plan (Step 1, awaiting approval)

Nothing below is built yet. Research (tools, prices, law) happens in Step 2.

## A. Design decisions (change any you disagree with)

| Decision | Choice | Why |
|---|---|---|
| Teaching mode | Claude runs lessons interactively, one at a time, and saves progress to `progress.md` | You asked for "never a dump". The skill is a tutor, not a book. |
| Stack | Python 3.12, FastAPI, SQLAlchemy 2 + Alembic, Pydantic | Fits your Python path. FastAPI is small enough to understand. Django is explained as the alternative in Module 4. |
| Database | SQLite for the first lessons, switch to PostgreSQL in Module 3 | Zero setup at the start, with the same SQL ideas. Postgres (Docker, or Neon/Supabase free tier) arrives before anything depends on it. |
| Frontend | Jinja templates + HTMX + a little vanilla JS (kanban via drag-drop) | No Node or React build chain for a Python learner. React is covered as "when to graduate". |
| Mini-CRM | "Hearth CRM": an estate-agency CRM (properties, applicants, viewings, pipeline) | It is your real project #1. Each module ends with a "swap the vertical" box showing how the same engine becomes the hospital CRM (patients, appointments, consent). |
| AI layer | Anthropic API via the official SDK, plus pgvector for embeddings | Tool use, evals, cost control and human approval are all taught on the mini-CRM. Other providers are noted. |
| Honesty | Every fact in `references/` carries a "verified on <date>, source <URL>, confidence" tag. A `known-unknowns.md` lists what is uncertain or fast-moving. | Prices, portal APIs and law change. |

## B. Skill folder layout (what Step 2 produces)

```
crm-builder-academy/
├── SKILL.md                    # tutor protocol, triggers, lesson index, progress rules (<500 lines)
├── references/
│   ├── teaching-protocol.md    # lesson template, level check, pacing/adaptation rules, quiz format
│   ├── glossary.md             # every term in plain English (first-use definitions)
│   ├── lessons/M00..M14-*.md   # one file per module; each holds its lessons
│   ├── architecture.md         # whole-system diagrams (ASCII/Mermaid)
│   ├── tools-and-costs.md      # researched: hosting, email, WhatsApp, AI APIs, portals, buy-vs-build
│   ├── uk-gdpr-estate-agency.md
│   ├── india-dpdp-health.md
│   ├── portals-uk.md           # Zoopla / OnTheMarket / OpenRent feed reality
│   ├── security-checklist.md
│   ├── roadmap-and-estimates.md
│   └── known-unknowns.md
├── exercises/
│   └── M00..M14/               # exercise.md + starter code + tests + solution (hidden until you ask)
├── mini-crm/                   # checkpoint code per module (hearth-crm-m03/, m04/ ...)
├── scripts/                    # check_exercise.py (runs the tests), progress helper
└── evals/                      # skill-creator test cases
```

## C. Generator prompt (the brief used to build every lesson file)

> You are writing one lesson of "crm-builder-academy" for a developer who builds things themselves, is learning Python step by step (has done file downloads, HTTP requests, multi-threading concepts), knows WordPress/Elementor/Hostinger, and knows **nothing** about CRM design.
>
> **Rules**
> 1. One lesson = 15-30 minutes. One idea. Never more than ~150 lines of code in total.
> 2. Define every CRM, database, web or AI term in plain words the first time it appears (bold it, add to glossary).
> 3. Fixed lesson skeleton, in this order: **Goal** → **Analogy** (everyday, ideally WordPress or estate-agency based) → **Why it exists** → **How it works** → **Diagram** (ASCII/Mermaid, shows the real mechanism) → **Working Python** (runs as-is, uses only what earlier lessons introduced, plus one new thing) → **Hands-on exercise** (a small change to the mini-CRM, with a test that passes when done) → **3 check questions** (one recall, one apply, one "what breaks if…") with answers hidden until the learner answers → **Common mistakes** → **Vertical swap** (estate agency vs hospital) → **Next**.
> 4. The code is added to the single growing Hearth CRM. State which files change, and which checkpoint it starts from and ends at.
> 5. Facts about prices, laws, APIs and limits must come from `references/` files verified in Step 2. Never write them from memory. If unsure, say "unsure" and point to `known-unknowns.md`.
> 6. Law content: summarise, cite the source, and end with "Ask a UK solicitor / Indian counsel before relying on this."
> 7. Be honest about what is hard (e.g. WhatsApp approval, portal feeds, consent), what is outdated, and where the lesson simplifies.
> 8. Tone: patient, concrete, no jargon without a definition, no padding.
>
> **Tutor behaviour (goes in SKILL.md)**
> - Start with a 6-question level check (Python, SQL, web, git, terminal, CRM knowledge). Set the pace as fast, normal or gentle, and skip or compress lessons accordingly.
> - Teach one lesson, then stop. Wait for the exercise result and the check-question answers. Re-explain differently if they are wrong, and don't just repeat the same text.
> - Update `progress.md` (module, lesson, scores, weak topics) after every lesson.
> - Never dump a module. Offer "next lesson / review / deeper dive / skip".

## D. Lesson map

Pace: about 45 lessons, roughly 15-30 minutes each (about 15-20 hours of learning in total). "Checkpoint" is the state of the Hearth CRM at the end of the module.

| # | Module | Lessons | Outcome | Hearth checkpoint |
|---|---|---|---|---|
| 0 | **Level check & orientation** | 0.1 level check · 0.2 what we'll build · 0.3 setup (Python, venv, git, editor) | Calibrated pace, working environment | Empty repo + health endpoint |
| 1 | **What a CRM is** (topic 1) | 1.1 the problem a CRM solves (spreadsheet → CRM) · 1.2 kinds: sales, support, marketing, vertical · 1.3 anatomy of any CRM (5 layers) · 1.4 build vs buy vs customise (Salesforce, HubSpot, Pipedrive, Reapit, Alto, Jupix, Practice Fusion-type HIS) | Can explain a CRM and place the 3 projects among the kinds | Paper design of Hearth |
| 2 | **Data model** (topic 2) | 2.1 contacts & companies · 2.2 leads vs deals vs applicants · 2.3 pipelines & stages · 2.4 activities, tasks, notes (the timeline) · 2.5 properties/patients as vertical objects · 2.6 drawing the entity diagram | Can design an entity-relationship diagram for any vertical | ER diagram + dataclass prototype in plain Python |
| 3 | **Database with PostgreSQL** (topic 3) | 3.1 tables, rows, keys, SQL basics · 3.2 relationships (1-many, many-many, join tables) · 3.3 SQLAlchemy models & migrations · 3.4 indexes & query plans · 3.5 moving to PostgreSQL, constraints, JSONB custom fields · 3.6 seeding realistic data | Working schema, queried from Python | Hearth DB v1 on Postgres |
| 4 | **Backend & API** (topic 4) | 4.1 what an API is (REST, JSON, status codes) · 4.2 first FastAPI routes · 4.3 CRUD for contacts · 4.4 validation, errors, pagination · 4.5 FastAPI vs Django decision | A tested CRUD API | Hearth API v1 |
| 5 | **Auth, roles & permissions** (topic 4) | 5.1 passwords, sessions vs tokens · 5.2 login & hashing · 5.3 roles, ownership, row-level access · 5.4 multi-user rules (agent sees own, manager sees team) | Secure login and role checks | Hearth with users and roles |
| 6 | **Frontend** (topic 5) | 6.1 server-rendered pages & HTMX · 6.2 list view with sort/filter/search · 6.3 forms & validation UX · 6.4 kanban pipeline (drag a deal between stages) · 6.5 contact timeline page | Usable UI | Hearth UI v1 |
| 7 | **Integrations** (topic 6) | 7.1 webhooks & polling, API keys · 7.2 website forms → leads (your WordPress/Elementor site) · 7.3 email in/out (SMTP, IMAP, providers) · 7.4 WhatsApp Business Platform · 7.5 calendar (Google/Microsoft) & viewings · 7.6 call logs / telephony · 7.7 property portals: what is really possible (Zoopla, OnTheMarket, OpenRent; feeds vs APIs; agent software gatekeepers) | Know what to build vs buy per channel, with one working inbound integration | Hearth: web form + email-log + viewings calendar |
| 8 | **Automation** (topic 7) | 8.1 triggers/conditions/actions · 8.2 background jobs (queues, scheduler) · 8.3 reminders & follow-up rules · 8.4 safe automation (idempotency, retries, loop prevention) | Event-driven rules engine | Hearth rules engine |
| 9 | **Reports & dashboards** (topic 8) | 9.1 metrics that matter (pipeline value, conversion, response time) · 9.2 SQL aggregates & views · 9.3 dashboard page with charts | Manager dashboard | Hearth dashboard |
| 10 | **Data safety & operations** (topic 9) | 10.1 import/export (CSV, dedupe) · 10.2 audit logs · 10.3 backups & restore tests · 10.4 multi-company (tenancy models: shared schema vs separate DB) | Trustworthy data handling | Hearth with audit, import, tenants |
| 11 | **Cloud, deployment, security, cost** (topic 10) | 11.1 where it runs (VPS vs PaaS vs cloud; Hostinger's limits for this) · 11.2 Docker & deploy · 11.3 HTTPS, secrets, OWASP basics · 11.4 monitoring & logs · 11.5 real monthly costs at 3 sizes | A deployed, secured Hearth | Live URL |
| 12 | **AI layer** (topic 11) | 12.1 how LLM APIs work (tokens, prompts, cost) · 12.2 summaries & email drafting · 12.3 lead scoring (rules vs ML vs LLM) · 12.4 embeddings & semantic search with pgvector · 12.5 chat over CRM data (RAG, permissions-aware) · 12.6 call transcription · 12.7 next-best-action · 12.8 agents with tools · 12.9 human approval gates · 12.10 evaluation (test sets, regressions) · 12.11 cost control & privacy (caching, model tiers, redaction) | Working AI features, each with an eval and an approval gate | Hearth AI v1 |
| 13 | **Compliance** (topic 12) | 13.1 UK GDPR for estate agencies (lawful basis, consent vs legitimate interest, PECR marketing rules, DSARs, retention, ICO fee, anti-money-laundering records) · 13.2 India DPDP Act 2023 + 2025 Rules (consent, fiduciaries, children, breach) · 13.3 health-data rules in India (ABDM, clinical-establishment record retention, telemedicine, sector rules) · 13.4 AI-and-personal-data rules · 13.5 compliance checklist as code (consent fields, retention jobs, DSAR export) | Consent, retention and DSAR features built in, plus "ask a lawyer" checklist | Hearth compliance features |
| 14 | **Build roadmap** | 14.1 what to build first for each of the 3 projects · 14.2 buy vs build table (hosting, email, WhatsApp, AI, auth, payments) · 14.3 realistic cost and time estimates (solo dev) · 14.4 risks and what's hard · 14.5 capstone review | A written plan for your 3 projects | Final Hearth + roadmap |

## E. Research plan (Step 2, via deep-research)

Run as parallel research threads, each writing a sourced reference file with a verified date:
1. **UK portals**: Zoopla, OnTheMarket and OpenRent feeds and APIs (who may connect, cost, formats such as Rightmove-style RTDF/Reapit/Alto feeds).
2. **UK law**: UK GDPR and the Data (Use and Access) Act 2025 changes, PECR, ICO guidance for estate agents, AML supervision, Renters' Rights Act relevance to data.
3. **India law**: DPDP Act 2023 and DPDP Rules 2025 (phase-in dates), ABDM/health data, DISHA status, Telemedicine guidelines, record retention.
4. **Tools and costs**: Postgres hosting (Neon, Supabase, Railway, Hetzner, AWS), email (Resend, Postmark, SES), WhatsApp Cloud API pricing, telephony (Twilio, Exotel for India), transcription, Anthropic and other AI API pricing, pgvector, and whether Hostinger plans can run this.
5. **Framework state**: current FastAPI, SQLAlchemy, HTMX and Django versions and idioms. Outdated ones are flagged.

## F. Build and test plan (skill-creator)

- Write the skill, then 3 test prompts: (a) "start the course" (level check → lesson 1), (b) a learner says "I'm confident in SQL, skip ahead to the API", (c) "explain webhooks for portal enquiries".
- Run each with and without the skill. Check that the lesson follows the template, stays to one lesson, defines terms, and cites `references/` instead of guessing.
- Run every code block and exercise test for real. Nothing ships that doesn't execute.
- Then optimise the description for triggering and package the skill.

## G. What I need you to confirm

1. Stack OK (FastAPI + SQLAlchemy + Postgres + HTMX)? Or do you prefer Django?
2. Mini-CRM is estate-agency flavoured with hospital "vertical swap" notes. OK?
3. Is about 45 lessons too many? I can cut to about 30 by merging within modules.
4. Is Anthropic API fine for the AI layer examples (other providers noted)?
5. Where should the finished skill live: this repo at `crm-builder-academy/` on branch `claude/crm-builder-academy-skill-6rpuiz`?
