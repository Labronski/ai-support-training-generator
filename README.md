# AI Support Training Generator

An AI-powered tool that automatically generates comprehensive training materials for technical support agents by analyzing support conversation data and using LLMs to produce structured, ready-to-use training content.

Built as a hackathon project to solve the problem of keeping support training materials current as products evolve — instead of manually writing docs, this tool pulls real conversation data and generates them automatically.

---

## What It Does

Support agent training is time-consuming to create and quickly goes stale. This tool connects to your support platform (Intercom), pulls recent conversation data, categorizes it by product area, and uses AI to generate a full training package for each area — including foundational knowledge, worked examples, practice scenarios, quizzes, and escalation guidelines.

---

## Features

- Automated conversation fetching from Intercom with pagination and retry handling
- Categorization by product area using conversation attributes
- AI-generated training materials via OpenAI (GPT-4) or Anthropic (Claude)
- Five material types generated per area: foundational knowledge, handling examples, test scenarios, quizzes, and escalation guidelines
- Optional Confluence integration to enrich materials with existing documentation
- Timestamped output directories for versioning
- Fully configurable via environment variables

---

## Tech Stack

- **Python 3.8+**
- **OpenAI API** (GPT-4) or **Anthropic API** (Claude) — training material generation
- **Intercom REST API** — conversation data source
- **Confluence REST API** — optional documentation enrichment
- **python-dotenv** — environment configuration

---

## Project Structure

```
├── main.py                        # Main orchestration script
├── config.py                      # Configuration and environment variable loading
├── requirements.txt               # Python dependencies
├── .env.example                   # Example environment variables
├── .gitignore
├── src/
│   ├── intercom_api.py            # Intercom API integration
│   ├── ai_trainer.py              # AI training material generator
│   └── confluence_integration.py  # Optional Confluence integration
├── data/                          # Intermediate data storage (gitignored)
└── outputs/                       # Generated materials (gitignored)
    └── training_materials_YYYYMMDD_HHMMSS/
        ├── wallet/
        ├── swaps/
        ├── ramps/
        └── ...
```

---

## Setup

### Prerequisites

- Python 3.8+
- Intercom access token (read permissions on conversations)
- OpenAI or Anthropic API key

### Installation

```bash
git clone https://github.com/Labronski/ai-support-training-generator.git
cd ai-support-training-generator

pip install -r requirements.txt

cp .env.example .env
# Edit .env with your credentials
```

### Environment Variables

```bash
# Required
INTERCOM_ACCESS_TOKEN=your_intercom_token
OPENAI_API_KEY=your_openai_key

# Optional — use Claude instead of GPT-4
USE_ANTHROPIC=false
ANTHROPIC_API_KEY=your_anthropic_key

# Optional — Confluence integration
ENABLE_CONFLUENCE=false
CONFLUENCE_BASE_URL=https://your-domain.atlassian.net
CONFLUENCE_USERNAME=your_email@example.com
CONFLUENCE_API_TOKEN=your_confluence_token

# Configuration
DAYS_BACK=30
MAX_CONVERSATIONS_PER_AREA=100
LOG_LEVEL=INFO
```

---

## Usage

```bash
python main.py
```

The script will:
1. Fetch conversations from Intercom for the configured date range
2. Categorize them by product area
3. Generate training materials for each area using AI
4. Save output to `outputs/training_materials_[timestamp]/`

**Tip:** Start with `DAYS_BACK=7` for a quick test run before processing larger datasets.

---

## Generated Material Types

For each product area, the tool generates five document types:

**Foundational Knowledge** — Core concepts, terminology, and product-specific fundamentals every agent needs to know.

**Handling Examples** — Real-world issue examples with recommended agent responses, key tips, and common pitfalls.

**Test Scenarios** — Realistic practice scenarios with expected agent actions and evaluation criteria.

**Quizzes** — Multiple choice, true/false, short answer, and scenario-based questions with full answer keys.

**Escalation Guidelines** — Clear criteria for when to escalate, what information to include, and what to try first.

---

## Configuration

Edit `config.py` to change which product areas are processed:

```python
PRODUCT_AREAS = [
    "Wallet",
    "Swaps",
    "Ramps",
    "Staking",
    "Card",
    "Perpetuals"
]
```

---

## Notes

- Generated `outputs/` and intermediate `data/` are gitignored — only source code is tracked
- The `.env` file is gitignored — never commit credentials
- Conversation data is processed in memory and not stored permanently
