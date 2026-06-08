# Bug Escalation - LangGraph + Swytchcode

Automates cross-platform bug escalation:
1. Creates a GitHub issue with severity label
2. Creates a linked Jira ticket
3. Notifies the engineering channel on Slack

Built with [LangGraph](https://github.com/langchain-ai/langgraph) and [Swytchcode](https://swytchcode.com).

---

## Setup

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Copy and fill in your API keys
cp .env.example .env

# 3. Fetch all integrations
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
