# Solar - AI-Powered Personal Operating System

> **Technical Challenge Submission**
> A conversational AI platform with multi-provider routing, persistent memory, and bidirectional transport architecture.
>
> **Built on [Solar AI OS](https://github.com/Uhorizon-AI/Solar)** - An open-source AI operating system

---

## 🎯 Problem Statement

Modern professionals need an AI assistant that:
- **Adapts to their workflow** (not the other way around)
- **Works where they already are** (Telegram, Slack, etc.)
- **Maintains conversation continuity** across sessions
- **Routes intelligently** between multiple AI providers for reliability and cost optimization

Most AI tools force users into new interfaces. Solar brings AI **to** the user's existing channels.

---

## 🏗 Architecture Overview

Solar uses a **Hub-and-Spoke** model with three core layers:

```
┌─────────────┐
│   Telegram  │  ← Frontend (conversational UI)
└──────┬──────┘
       │ webhook
       ▼
┌─────────────────┐
│ Cloudflare Tunnel│  ← Public endpoint + security
└──────┬──────────┘
       │
       ▼
┌──────────────────┐
│ Transport Gateway│  ← Local WebSocket + HTTP bridge
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│   AI Router      │  ← Multi-provider (Claude → Codex → Gemini)
└──────────────────┘
```

### Key Components

1. **solar-telegram** - Channel adapter for Telegram transport
2. **solar-transport-gateway** - Bidirectional message routing with WebSocket core
3. **AI Router** - Provider selection with automatic fallback
4. **Conversation Memory** - Persistent JSONL storage per user

---

## 🛠 Tech Stack

### Frontend
- **Telegram Bot API** - Conversational interface
- **Markdown formatting** - Rich message rendering
- **Webhook-based** - Real-time message delivery

### Backend
- **Python** (Poetry for dependency management)
- **WebSocket** - Bidirectional runtime transport
- **HTTP webhook bridge** - Channel adapter endpoints
- **JSONL storage** - Conversation persistence
- **Bash orchestration** - Setup and runtime management

### AI Integration
- **Anthropic Claude** (primary)
- **OpenAI Codex** (fallback)
- **Google Gemini** (fallback)
- **Provider priority** - Configurable via `SOLAR_AI_PROVIDER_PRIORITY`

### Infrastructure
- **Cloudflare Tunnel** - Secure public endpoint
- **Local runtime** - No external hosting required
- **Environment-based config** - `.env` for secrets

---

## 🤖 How AI is Used

### 1. Multi-Provider Routing with Fallback

Solar doesn't lock into a single AI provider. It uses a **priority queue** with automatic failover:

```bash
SOLAR_AI_PROVIDER_PRIORITY="claude,codex,gemini"
```

**Why this matters:**
- **Reliability** - If one provider is down, routing continues
- **Cost optimization** - Route simple queries to cheaper models
- **Provider flexibility** - Easy to add new providers

### 2. Conversation Continuity

Each user's conversation is stored locally as JSONL:

```
sun/runtime/transport-gateway/
  ├── conversations/
  │   ├── user_123456.jsonl
  │   └── user_789012.jsonl
```

**Benefits:**
- Context persists across sessions
- Users don't repeat themselves
- AI maintains conversation memory

### 3. System Prompt Customization

Solar loads a custom system prompt that defines:
- The Hub-and-Spoke architecture
- Routing protocols between "Sun" (personal agent) and "Planets" (domain agents)
- Governance rules per workspace

This transforms generic AI into a **domain-aware assistant**.

### 4. Why These Choices?

- **Claude** - Best reasoning and coding capabilities
- **Multi-provider** - Real-world reliability (APIs fail, rate limits hit)
- **Local-first** - User owns their data, no vendor lock-in
- **Conversation memory** - AI that actually remembers context

---

## 🎬 Demo

### Live Demo
[Watch Demo Video](https://www.loom.com/share/d8d360c2fe20414cb4bd3e3c138a0e3e)

### Screenshots
*Example Telegram conversation:*

```
User: "Need to migrate my existing repos into Solar"

Solar: "I'll help you migrate safely. Suggested approach:

1. Map each repo to core/sun/planets based on purpose
2. Create AGENTS.md per planet with governance rules
3. Migrate in prioritized batches (critical → automation → cleanup)
4. Validate end-to-end flow per planet

Which 3 repos should we start with?"
```

---

## 🚀 Setup Instructions

### Prerequisites
- Python 3.9+
- Poetry
- Telegram Bot Token
- At least one AI provider CLI (`claude`, `codex`, or `gemini`)

### 1. Clone Repository

```bash
git clone https://github.com/yourusername/solar.git
cd solar
```

### 2. Configure Environment

```bash
# Copy example env
cp .env.example .env

# Add your credentials
TELEGRAM_BOT_TOKEN=your_bot_token
TELEGRAM_CHAT_ID=your_chat_id
SOLAR_AI_PROVIDER_PRIORITY=claude,codex,gemini
```

### 3. Run Setup

```bash
# Full setup (one command)
bash core/skills/solar-transport-gateway/scripts/setup_transport_gateway.sh
```

This will:
- Install Python dependencies
- Validate environment variables
- Start local WebSocket server
- Start Cloudflare tunnel
- Register Telegram webhook
- Start AI routing loop

### 4. Test

Send a message to your Telegram bot. You should receive an AI-powered response within seconds.

---

## ⚖️ Trade-offs & Design Decisions

### What Went Well

- **Local-first architecture** - No vendor lock-in, full data ownership
- **Multi-provider routing** - Real-world reliability and cost optimization
- **Conversational UX** - Users stay in their existing tools (Telegram)
- **Modular design** - Skills are reusable, channels are pluggable

### Trade-offs Made

#### 1. Local Runtime vs Cloud Hosting
- **Chose:** Local runtime on laptop
- **Why:** Faster development, no hosting costs, full control
- **Trade-off:** Requires laptop to be awake for message delivery
- **Future:** Deploy gateway to Railway/Fly.io for 24/7 uptime

#### 2. JSONL vs Database
- **Chose:** JSONL flat files for conversation storage
- **Why:** Simple, fast, no database dependencies
- **Trade-off:** Not ideal for complex querying or multi-user scale
- **Future:** Migrate to PostgreSQL or SQLite for search/analytics

#### 3. Webhook vs Polling
- **Chose:** Telegram webhooks (not polling)
- **Why:** Real-time delivery, lower latency, cleaner architecture
- **Trade-off:** Requires public endpoint (Cloudflare tunnel)
- **Future:** Already optimal for this use case

#### 4. Single System Prompt vs Dynamic Context
- **Chose:** One shared system prompt file
- **Why:** Consistency across conversations, easier to maintain
- **Trade-off:** Doesn't adapt per-user preferences yet
- **Future:** Load user-specific context from `sun/preferences/`

### What I'd Improve with More Time

1. **Authentication** - Add user verification for Telegram webhook
2. **Rate limiting** - Protect against spam/abuse
3. **Testing** - Unit tests for AI router, integration tests for gateway
4. **Monitoring** - Structured logging + error alerting (Sentry)
5. **Web UI** - Admin dashboard for conversation history and analytics
6. **Multi-channel** - Add Slack, Discord, WhatsApp adapters
7. **Deployment** - Docker containerization + one-click Railway deploy

---

## 📊 Evaluation Criteria Mapping

| Criterion | How Solar Demonstrates This |
|-----------|------------------------------|
| **Problem-solving** | Solves real UX problem (AI in existing tools) |
| **Full-stack** | Frontend (Telegram) + Backend (Gateway) + AI |
| **AI usage** | Multi-provider routing with conversation memory |
| **Code quality** | Modular skills, clear separation of concerns |
| **Decision clarity** | Documented trade-offs and design rationale |
| **Product/UX** | Conversational interface, persistent context |

---

## 📁 Project Structure

```
solar/
├── core/
│   ├── skills/
│   │   ├── solar-telegram/          # Telegram channel adapter
│   │   └── solar-transport-gateway/ # WebSocket + routing layer
│   └── scripts/                      # Shared utilities
├── sun/
│   ├── preferences/                  # User profile
│   └── runtime/                      # Conversation memory
└── planets/
    └── solar-challenge/              # This submission
```

---

## 🔗 Links

- **GitHub Repository:** [louisjimenezp/solar-challenge](https://github.com/louisjimenezp/solar-challenge)
- **Demo Video:** [Watch on Loom](https://www.loom.com/share/d8d360c2fe20414cb4bd3e3c138a0e3e)

---

## 📝 Notes

- **Time invested:** ~6 hours (setup 2h, architecture 2h, documentation 2h)
- **AI providers tested:** Claude Opus 4.6, OpenAI GPT-5.3-Codex

---

**Built with ❤️ for the Indigo AI Technical Challenge**
