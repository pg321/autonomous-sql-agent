# Autonomous SQL & API Agent

An autonomous natural language to SQL query agent built with LangChain, Python, and SQLAlchemy. The agent dynamically inspects database schemas, translates natural language questions into dialect-aware SQL queries, validates queries against destructive operations, and employs a self-correcting feedback loop to automatically recover from execution errors before returning results.

---

## Features

- **Natural Language to SQL Conversion:** Translates complex analytical questions (JOINs, aggregations, groupings, subqueries) into precise SQL queries.
- **Dynamic Schema Inspection:** Uses SQLAlchemy reflection to fetch table structures, column types, and sample data dynamically, preventing hallucinated tables and columns.
- **Automatic Query Validation & Error Recovery:** Intercepts database exceptions or syntax errors and passes them back into the LLM context to automatically refine, correct, and re-execute the query.
- **Safety & Guardrails:** Enforces read-only database execution, blocking destructive operations (`DROP`, `DELETE`, `UPDATE`, `INSERT`, `ALTER`, `TRUNCATE`).
- **Runaway Query Protection:** Restricts default result sets (`top_k` limiting) and applies execution boundaries to prevent heavy runaway operations.
- **Observability & Tracing:** Integrated with LangSmith for tracing tool calls, token usage, execution latency, and retry cycles.

---

## Demo Database

Uses the **Chinook database**, a sample relational database representing a digital media store with tables for artists, albums, tracks, customers, invoices, and employees.

---

## Quick Start

### Prerequisites
- Python 3.10 or higher
- OpenAI or Anthropic API key
- (Optional) LangSmith API key for execution tracing

### Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/pg321/autonomous-sql-agent.git](https://github.com/pg321/autonomous-sql-agent.git)
   cd autonomous-sql-agent
Download the Chinook sample database:

Bash
curl -L -o chinook.db [https://github.com/lerocha/chinook-database/raw/master/ChinookDatabase/DataSources/Chinook_Sqlite.sqlite](https://github.com/lerocha/chinook-database/raw/master/ChinookDatabase/DataSources/Chinook_Sqlite.sqlite)
Create a virtual environment and install dependencies:

Bash
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
Configure environment variables:

Bash
cp .env.example .env
Add your keys in .env:

Ini, TOML
ANTHROPIC_API_KEY=your_anthropic_api_key_here
# Optional: LangSmith Tracing
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=your_langsmith_api_key_here
LANGCHAIN_PROJECT=autonomous-sql-agent
Usage
Command Line Interface
Ask questions against your database directly in plain English:

Bash
python agent.py "What are the top 5 best-selling artists?"
python agent.py "Which employee generated the most revenue?"
python agent.py "How many customers are from Canada?"
Interactive Tutorial
Explore the step-by-step construction, schema reflection, and agent decision loops:

Bash
jupyter notebook tutorial.ipynb
How It Works
The agent uses LangChain with the following execution loop:

Discover: Lists available tables in the connected database catalog.

Inspect: Retrieves schema definitions and sample rows for relevant tables via SQLAlchemy.

Generate: Creates a dialect-specific SQL query using LLM tool calling.

Validate: Enforces read-only safety guardrails (SELECT only).

Execute: Runs the query against the database.

Retry: If execution throws an error, the exception context is passed back to the LLM to rewrite and retry the query.

Format: Returns the verified final results in clear natural language.

Project Structure
Plaintext
autonomous-sql-agent/
├── agent.py              # Core agent implementation & execution loop
├── tutorial.ipynb        # Interactive step-by-step tutorial notebook
├── chinook.db           # Sample SQLite database
├── pyproject.toml       # Project configuration and dependencies
├── uv.lock              # Dependency lockfile
├── .env.example         # Environment variable template
├── .gitignore           # Git ignore rules
└── README.md            # Project documentation
License
MIT
