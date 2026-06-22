# Bug Escalation — LangGraph + Swytchcode

Automates cross-platform bug escalation:
1. Creates a GitHub issue with severity label
2. Creates a linked Jira ticket
3. Notifies the engineering channel on Slack

Built with [LangGraph](https://github.com/langchain-ai/langgraph) and [Swytchcode](https://swytchcode.com).

---

## Prerequisites

- **Python 3.9+**
- **Swytchcode CLI:** install with the verified script for your platform:
  
  npm install -g swytchcode

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
## Run

```bash
python main.py
```

## Canonical IDs Used

| Service | Canonical ID                               |
|---------|--------------------------------------------|
| GitHub  | `repos.issue.create`                       |
| Jira    | `rest.api.issue.create`                    |
| Slack   | `chat.postmessage.chat.postmessage.create` |
