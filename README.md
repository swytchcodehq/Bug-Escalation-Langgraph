# Bug Escalation (LangGraph + Swytchcode)

A LangGraph agent that escalates a reported bug across GitHub, Jira, and Slack in one run.

> Run one command to open a GitHub issue, file a linked Jira ticket, and post the alert to Slack, without writing API glue code or managing credentials and retries.

[![Python 3.9+](https://img.shields.io/badge/python-3.9%2B-blue?style=flat-square)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Last commit](https://img.shields.io/github/last-commit/swytchcodehq/Bug-Escalation-Langgraph?style=flat-square)](https://github.com/swytchcodehq/Bug-Escalation-Langgraph/commits)

## What this does

This demo escalates a bug across three tools at once. It opens a GitHub issue with a severity label, creates a Jira ticket that links back to that issue, and posts a severity-coded notification to a Slack channel. The steps run as a LangGraph state machine, so the GitHub issue URL flows into both the Jira ticket and the Slack message.

Every external call goes through [Swytchcode](https://www.swytchcode.com/), a deterministic API execution layer for AI agents. The agent code never calls GitHub, Jira, or Slack directly. It asks the Swytchcode runtime to run a named method, and the runtime validates the request against a schema registry of 2,000+ integrations, handles auth and retries, and records an audit trail of what ran.

## How it works

The graph has three nodes and runs them in order:

```
create_github_issue -> create_jira_ticket -> notify_slack
```

- **create_github_issue** opens an issue in the target repo via `repos.issue.create`, with the title prefixed by severity and labeled with the severity plus `bug`.
- **create_jira_ticket** creates a Jira task via `rest.api.issue.create`, with a description that links back to the GitHub issue. Auth is HTTP Basic, built from the Jira email and API token.
- **notify_slack** posts a severity-coded message to the Slack channel via `chat.postmessage.chat.postmessage.create`, including the GitHub and Jira references.

## Prerequisites

- **Python 3.9+**
- **Swytchcode CLI:** install with the verified script for your platform:

  Linux / macOS:
  ```bash
  curl -fsSL https://cli.swytchcode.com/install.sh | sh
  ```
  Windows (PowerShell):
  ```powershell
  irm https://cli.swytchcode.com/install.ps1 | iex
  ```
- A **GitHub** token, **Jira** credentials, and a **Slack** bot token (see the table below).

## Setup

1. Clone the repo:
   ```bash
   git clone https://github.com/swytchcodehq/Bug-Escalation-Langgraph.git
   cd Bug-Escalation-Langgraph
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Copy the example env file and fill in your keys:
   ```bash
   cp .env.example .env
   ```
4. Fetch the integrations declared in `.swytchcode/tooling.json`:
   ```bash
   swytchcode bootstrap
   ```

## Environment variables

| Variable | Required | Description |
|----------|----------|-------------|
| `GITHUB_TOKEN` | Yes | GitHub token with permission to create issues in the target repo. |
| `GITHUB_OWNER` | No | Owner of the target repo. Falls back to a sample value in `main.py` if unset. |
| `GITHUB_REPO` | No | Target repo name. Falls back to a sample value in `main.py` if unset. |
| `JIRA_API_TOKEN` | Yes | Jira API token. |
| `JIRA_EMAIL` | Yes | Jira account email, paired with the token for Basic auth. |
| `JIRA_BASE_URL` | Yes | Jira site base URL (`https://yourcompany.atlassian.net`). |
| `JIRA_PROJECT_KEY` | Yes | Project key the ticket is created under (for example `ENG`). |
| `SLACK_BOT_TOKEN` | Yes | Slack bot token (`xoxb-...`). |
| `SWYTCHCODE_TOKEN` | Yes | Swytchcode auth token. Get yours from the Swytchcode dashboard under Settings, API keys. |

The bug details (title, description, severity, Slack channel) are defined as a sample in `main.py`. Edit them there to escalate a different bug.

## Run

```bash
python main.py
```

## Expected output

The script prints each node as it runs and a summary at the end:

```
[1/3] Creating GitHub issue: Login page crashes on mobile Safari...
    GitHub issue created: #42 - https://github.com/owner/repo/issues/42
[2/3] Creating Jira ticket for GitHub issue #42...
    Jira ticket created: ENG-123
[3/3] Notifying Slack channel #general...
    Slack notified

Bug escalated across all platforms!
   GitHub Issue:   https://github.com/owner/repo/issues/42
   Jira Ticket:    ENG-123
   Slack notified: True
```

After a run you should see a new labeled issue in the GitHub repo, a Jira task linking back to it, and a severity-coded message in the Slack channel.

## Canonical IDs used

| Service | Canonical ID |
|---------|--------------|
| GitHub | `repos.issue.create` |
| Jira | `rest.api.issue.create` |
| Slack | `chat.postmessage.chat.postmessage.create` |

## Part of the Swytchcode demo collection

Runnable LangGraph + Swytchcode examples:

- [Weekly-Reporting-Langgraph](https://github.com/swytchcodehq/Weekly-Reporting-Langgraph)
- [Customer-Onboarding-Langgraph](https://github.com/swytchcodehq/Customer-Onboarding-Langgraph)
- [Create-And-Send-Payment-Langgraph](https://github.com/swytchcodehq/Create-And-Send-Payment-Langgraph)
- [Lead-Qualification-Langgraph](https://github.com/swytchcodehq/Lead-Qualification-Langgraph)

## License

MIT. See [LICENSE](LICENSE).
