# n8n Workflow: Triage GitHub Issues to a Notion Board with Qwen

> 🏆 **n8n Verified Creator Workflow**  
> This workflow has been reviewed and approved by the n8n team.  
> 👤 Creator: [Do Thanh Vinh (Kevin Do)](https://github.com/dothanhvinh17)  
> 🔗 Live demo & docs: [vinhautomation.com/en/tools/github-notion-triage-qwen](https://www.vinhautomation.com/en/tools/github-notion-triage-qwen/)  
> ⬇️ Download workflow: [n8n.io/workflows/15755](https://n8n.io/workflows/15755-triage-github-issues-to-a-notion-board-with-qwen-via-openrouter/)

## 🎯 What It Does

This workflow automates GitHub issue triage using AI. When a new issue is opened:

1. **Classifies** it by type: bug, feature, question, documentation, other
2. **Assigns** a priority level: high, medium, low
3. **Generates** a one-sentence summary
4. **Writes** results to a Notion database + posts a comment on the GitHub issue

**Perfect for:** Teams who want to eliminate 30-60 seconds of manual sorting per issue while keeping humans in the loop for final decisions.

[GitHub Issue Opened] → [Duplicate Check] → [AI Triage] → [Notion + GitHub Comment]
↓
[Fallback: Default Values if LLM fails]

🚀 **Key Highlight:** AI classifies but does not decide — it does not close issues or assign people. A human reviews the Notion board and decides what to act on.

**🔗 See a live Notion board populated by this workflow:** [Notion Demo Board](https://www.notion.so/360e7dab6a3080098c7efd3f638b1915?v=360e7dab6a30801896bd000cc9e7206f)

## ✨ Key Features

### 🔹 Smart AI Triage with Structured Prompts
- Prompt includes detailed definitions for each issue type and priority level → consistent classification
- If LLM call fails, fallback node assigns default values (category: other, priority: medium) → issue still tracked in Notion, never lost

### 🔹 Duplicate Check & Robust Error Handling
- Queries Notion by Issue URL before processing → avoids duplicate records + wasted API calls
- If duplicate check fails (timeout, rate limit), error branch fires and workflow continues → over-triaging is better than missing an issue

### 🔹 Results Published to Both Notion & GitHub
- GitHub comment displays category, priority, summary, suggested labels
- Notion database serves as centralized view — filter, sort, assign without switching tools
- If fallback values were used, comment includes warning so team knows to review manually

### 🔹 Private & Self-Hosted
- Issue data never leaves your infrastructure
- Only external call goes to LLM provider (OpenRouter by default) — can be replaced with self-hosted model for full internal control

## 🚀 Quick Start

### ⚙️ Prerequisites
- ✅ n8n instance (self-hosted or cloud)
- ✅ GitHub Fine-grained Personal Access Token (Issues read/write, Webhooks read/write)
- ✅ Notion account with internal integration
- ✅ OpenRouter API Key (or any OpenAI-compatible endpoint)

### Steps:

1. **Configure GitHub**: Create Fine-grained PAT with Issues read/write + Webhooks read/write → Add GitHub credential in n8n

2. **Create Notion Database**: Create a Notion database with these properties:
   | Property | Type | Purpose |
   |----------|------|---------|
   | Title | title | Issue title |
   | Issue URL | url | Link back to GitHub |
   | Author | rich_text | Issue author |
   | Category | select | bug, feature, question, documentation, other |
   | Priority | select | high, medium, low |
   | Summary | rich_text | AI-generated one-liner |
   | Suggested Labels | rich_text | Labels AI recommends |
   | Status | select | Open, Closed |
   | Source | select | ai, fallback |
   | Created | date | Timestamp |

3. **Connect Notion**: Create integration at notion.so/my-integrations → Add Notion credential in n8n → Share database with integration (Database → ... → Connections)

4. **Configure Nodes**: 
   - "When Issue Opened": Set Owner + Repository to your GitHub repo
   - "Verify Notion Duplicate" + "Add to Notion Board": Set Database to your Notion DB
   - "OpenAI Qwen-3 Model": Add your OpenRouter API key

5. **Activate**: Enable workflow → Open a test issue on your GitHub repo to verify

## 🧩 Customization Options

- **Swap the LLM**: Change model in "OpenAI Qwen-3 Model" node to any OpenRouter-supported model (GPT-4o, Claude, Gemini) or point to self-hosted model
- **Adjust triage rules**: Edit prompt in "Initiate AI Triage" to add custom categories (e.g., "performance", "security")
- **Multi-repo**: Duplicate workflow for each repo, or modify trigger to use GitHub App watching multiple repos
- **Add team notifications**: After "Post GitHub Comment", add Slack/Discord node for high-priority alerts
- **Auto-assign**: Add logic to assign GitHub issues based on category (bugs → backend, docs → content)
- **Auto-label**: Use AI-suggested labels to automatically apply labels on GitHub issue (create labels in repo first)

## 📈 What's Next? (Extending This Workflow)

This workflow is intentionally simple — simple means cheap, stable, easy to debug. When your use case grows, consider:

| Enhancement | When to Add |
|-------------|-------------|
| **Auto-label on GitHub** | Labels created in repo → remove manual labeling step |
| **Batch Triage** | Process existing untriaged issues on schedule, not just new webhooks |
| **Priority-based Routing** | High priority → Slack/Discord alert; Medium/Low → Notion only |
| **Learning from Feedback** | Track human corrections in Notion → use as few-shot examples to improve AI accuracy |
| **Notion Dashboard** | Build filtered views by category/priority + "Triage backlog" sorted by priority |
| **Multi-source Triage** | Extend trigger to GitLab issues, Linear tickets, or support emails → same Notion board |

## ⚙️ Requirements Summary

- n8n instance (self-hosted or cloud)
- GitHub Fine-grained PAT (Issues read/write, Webhooks read/write)
- Notion account with internal integration
- OpenRouter API key (or any OpenAI-compatible endpoint)

## 💡 Cost Note

- ✅ Duplicate check: **Free** (prevents wasted API calls)
- ⚠️ LLM tokens: **Billed by OpenRouter** per new issue
- 📊 For repos with low-moderate issue frequency, cost per triage is minimal (~$0.001-0.01)

> Monitor usage via OpenRouter dashboard if running on high-volume repos (100+ issues/day).

## 🤝 Contributing & Feedback

- Found a bug in the anonymized example? → Open an issue
- Have an improvement idea? → Submit a PR
- Want to discuss a similar use case? → Email [vinh@vinhautomation.com](mailto:vinh@vinhautomation.com)

## 📄 License

MIT License — Free to use, modify, and share for learning purposes.  
Not for commercial resale without prior agreement.

---

🔗 **More by this creator:**  
[github.com/dothanhvinh17](https://github.com/dothanhvinh17) \| [vinhautomation.com/en/tools](https://www.vinhautomation.com/en/tools) \| [n8n.io/creators/dothanhvinh](https://n8n.io/creators/dothanhvinh)
