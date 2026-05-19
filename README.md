# Smart Metrics AI

> Advanced Agile Engineering Intelligence for Jira Cloud — built natively on Atlassian Forge.

[![Atlassian Forge](https://img.shields.io/badge/Atlassian-Forge-0052CC?logo=atlassian&logoColor=white)](https://developer.atlassian.com/platform/forge/)
[![Jira Cloud](https://img.shields.io/badge/Jira-Cloud-0052CC?logo=jira&logoColor=white)](https://www.atlassian.com/software/jira)
[![i18n](https://img.shields.io/badge/languages-6-brightgreen)](locales/)
[![Version](https://img.shields.io/badge/version-4.0.0-blue)](manifest.yml)

---

## What is Smart Metrics AI?

**Smart Metrics AI** is a Jira Cloud app that transforms your project data into actionable engineering intelligence. It calculates Lead Time, Cycle Time, Throughput, Friction Index, VSM, Monte Carlo forecasts and much more — all directly inside Jira, with zero external database or infrastructure required.

Built entirely on **Atlassian Forge**, all data stays within your Atlassian tenant. No external API calls, no data leaving your environment.

---

## Features

### Overview Dashboard
- **Lead Time & Cycle Time** (average + P85 percentile) with historical trend comparison
- **Throughput** (issues delivered per week)
- **Say/Do Ratio** — sprint commitment fulfillment
- **Story Points per Dev** — aggregate capacity indicator
- **Friction Index** — rework time as % of productive time
- **Avg. Blocked Time** — measures external dependencies
- **Bugs in Sprint** — quality trend over time
- **Scope Creep** monitoring — SP added/removed after sprint start

### Effort X-Ray Tab
- Friction Index with formula breakdown
- Bug Leakage by origin (sprint / production / environment)
- Defect Density (bugs per Story Point)
- Top 5 Offender Stories ranked by friction
- SP × Bugs scatter correlation with trend line

### Cycle Time Scatter Chart
- Lead Time × Cycle Time scatter with 4-quadrant analysis
- Flow Zone / Process Bottleneck / Technical Bottleneck / High Risk quadrants
- Bubble size = Story Points
- Regression trend line

### Probabilities Tab
- Completion probability by Fibonacci size (1–21 SP) within 10 business days
- Based on real team history (outlier-filtered)
- Quarterly capacity projection factoring Friction Index

### Monte Carlo Simulation
- 10,000-scenario simulation using historical daily throughput
- P50, P85, P95 confidence intervals for backlog delivery dates
- Trend warning if throughput is declining

### Value Stream Mapping (VSM)
- Drag-and-drop phase editor (map Jira statuses to flow phases)
- Health score per phase (green / yellow / red)
- Always uses Accumulated Time for accurate phase-by-phase analysis
- Bottleneck identification across the full flow

### Active Sprint Tab
- Real-time burndown chart (ideal vs. actual)
- Sprint health score (Scope Creep + open bugs + elapsed time)
- Featured issue highlights
- At-risk issue detection (blocked / stale / over P85)

### Built-in Metrics Manual
- In-app interactive guide covering all 7 feature areas
- Sidebar navigation, formulas, examples, tips
- Available in all 6 supported languages

---

## Internationalization

Smart Metrics AI is fully localized in **6 languages**:

| Language | Locale |
|---|---|
| English | `en-US` |
| Portuguese (Brazil) | `pt-BR` |
| Spanish | `es-ES` |
| French | `fr-FR` |
| German | `de-DE` |
| Japanese | `ja-JP` |

The app automatically uses the language set in the user's Jira instance.

---

## Requirements

- **Jira Cloud** (any plan)
- Atlassian Forge runtime (managed automatically — no server required)
- Admin access to install the app and configure projects

---

## Installation

1. Go to the [Atlassian Marketplace](https://marketplace.atlassian.com) and search for **Smart Metrics AI**.
2. Click **Get it now** and select your Jira Cloud site.
3. Follow the installation prompts and grant the requested permissions.

---

## Configuration

After installation:

1. Open Jira and navigate to **Apps → Smart Metrics AI → Configuration** (requires Jira Admin).
2. Click **Add Project** and select a Jira project.
3. Map your Jira statuses:
   - **Done Statuses** — statuses that represent completed work (e.g., `Done`, `Closed`).
   - **Active Statuses** — statuses that represent work in progress (e.g., `In Progress`, `Doing`).
4. Choose the **Cycle Time Strategy**:
   - **Total Span** (default) — first active status to last Done. Best for delivery predictability.
   - **Accumulated** — sum of active periods only. Best for measuring pure engineering effort.
5. Optionally assign team member names for the Points per Dev calculation.
6. Save. The app will automatically sync your project data in the background.

### Sync Behavior

- **First load**: the app bootstraps data automatically on first access.
- **Scheduled sync**: daily incremental sync runs automatically.
- **Real-time**: issue create/update/delete events trigger instant cache updates.
- **Manual sync**: use the Sync button in the dashboard header to force a full refresh.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   Atlassian Forge Runtime                │
│                                                         │
│  ┌──────────────┐    ┌──────────────┐    ┌───────────┐ │
│  │  Custom UI   │    │  Resolvers   │    │ Triggers  │ │
│  │  (React +    │◄──►│  (Backend    │    │ Scheduled │ │
│  │   Vite)      │    │   Functions) │    │ Product   │ │
│  └──────────────┘    └──────┬───────┘    │ WebTrigger│ │
│                             │            └─────┬─────┘ │
│                      ┌──────▼───────┐          │       │
│                      │ Forge Entity │◄─────────┘       │
│                      │   Storage    │                   │
│                      │  (KVS + ES)  │                   │
│                      └──────┬───────┘                   │
└─────────────────────────────┼───────────────────────────┘
                              │ Jira REST API
                    ┌─────────▼─────────┐
                    │   Jira Cloud      │
                    │  (Issues, Boards, │
                    │   Sprints, Users) │
                    └───────────────────┘
```

**Key design decisions:**
- **Storage-first**: metrics are calculated from cached issues in Forge Entity Storage, not fetched live on every load. This removes the 500-issue and 6-month limitations of live-fetch approaches.
- **Incremental sync**: daily trigger + product trigger (issue events) keep the cache fresh with minimal API calls.
- **No external services**: zero databases, zero external APIs, zero data leaving the Atlassian tenant.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Atlassian Forge (Node.js 22.x) |
| Frontend | React 18 + Vite + TypeScript |
| Charts | Recharts |
| Drag & Drop | @dnd-kit |
| i18n | react-i18next + Forge translations |
| Storage | Forge Entity Storage + KVS |
| Utilities | date-fns |

---

## Development

### Prerequisites

- [Node.js 18+](https://nodejs.org/)
- [Forge CLI](https://developer.atlassian.com/platform/forge/getting-started/)
- An Atlassian Cloud developer account

### Setup

```bash
# Install backend dependencies
npm install

# Install frontend dependencies
cd static/main && npm install && cd ../..

# Build frontend
cd static/main && npm run build && cd ../..

# Deploy to development environment
forge deploy

# Install on your dev site
forge install
```

### Development Tunnel (hot reload)

```bash
forge tunnel
```

### Running Tests

```bash
npm test
```

---

## Privacy & Security

Smart Metrics AI is built with privacy by design:

- **Data stays in your tenant**: all issue data is stored in Forge Entity Storage, which is scoped exclusively to your Atlassian site.
- **No external transmission**: the app makes no calls to external servers. All processing happens within the Forge runtime.
- **Minimal permissions**: the app only requests `read:jira-work`, `read:jira-user`, and `storage:app` — the minimum required to function.
- **Admin-controlled**: only Jira Admins can configure which projects are analyzed.

See [PRIVACY_POLICY.md](PRIVACY_POLICY.md) for full details.

---

## Support

- **Issues & feature requests**: open an issue in this repository
- **Marketplace support**: use the support link on the Atlassian Marketplace listing
- **Email**: [your-support-email@domain.com]

---

## License

See [TERMS_OF_SERVICE.md](TERMS_OF_SERVICE.md).

---

*Smart Metrics AI is an independent app and is not affiliated with or endorsed by Atlassian.*
