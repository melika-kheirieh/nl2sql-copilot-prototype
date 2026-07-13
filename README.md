---
title: NL2SQL Copilot
emoji: 🧠
colorFrom: indigo
colorTo: purple
sdk: gradio
python_version: "3.11"
app_file: app.py
pinned: false
---
# 🧠 NL2SQL Copilot — Archived Prototype

> **Archived prototype**
>
> This repository preserves the original v0.1 Gradio prototype of NL2SQL Copilot.
> Active development continued in the successor project:
> [melika-kheirieh/nl2sql-copilot](https://github.com/melika-kheirieh/nl2sql-copilot).
>
> This prototype is preserved for historical reference and is no longer actively maintained.

A minimal **Text-to-SQL Copilot** built with **LangChain and Gradio**.

It translates natural-language questions into guarded, read-only SQL queries, validates them before execution, and runs approved queries against an uploaded SQLite database.

👉 [Live Demo on Hugging Face Spaces](https://huggingface.co/spaces/melikakheirieh/nl2sql-copilot-prototype)

---

## What This Prototype Demonstrates

* Gradio-based interactive UI
* Natural-language-to-SQL generation with LangChain
* Configurable OpenAI-compatible LLM provider
* Uploaded SQLite database support
* Schema inspection before SQL generation
* SQL parsing with `sqlglot`
* Single-statement enforcement
* `SELECT`-only validation
* Blocking of forbidden SQL keywords and internal SQLite tables
* Read-only SQLite execution
* Configurable result row limits
* Environment-based secret management

This is an early prototype, not a production-grade SQL security boundary.

---

## Project Structure

```text
nl2sql-copilot-prototype/
├── app.py
├── config.py
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
└── db/
    ├── Chinook_Sqlite.sqlite
    └── WMSales.sqlite
```

---

## Database Samples

Two example SQLite databases are included in the `db/` directory for local testing.

| File                    | Description                                                                               |
| ----------------------- | ----------------------------------------------------------------------------------------- |
| `Chinook_Sqlite.sqlite` | A sample music-store database containing artists, albums, tracks, customers, and invoices |
| `WMSales.sqlite`        | A sample sales database for aggregation, filtering, and reporting queries                 |

You can upload either database through the Gradio interface or use another `.sqlite` or `.db` file.

---

## Example Questions

### Chinook Database

* List the top five artists by total track count.
* Which album contains the most tracks?
* Show all tracks longer than six minutes.
* Find the average track length by genre.
* Show total invoice revenue by billing country.
* List the ten most popular genres by number of tracks.
* How many customers purchased Jazz albums?
* Show total revenue by sales support employee.
* List customers who spent more than $100.
* Which customers are located in Canada?

### Sales Database

* Show total sales per month in 2024.
* List the top ten customers by revenue.
* Which product category had the highest sales?
* Find the average unit price per product.
* Show orders placed during the last 30 days.
* List total sales by region and salesperson.
* What is the best-selling product?
* Show the total discount given per month.
* Find customers who made more than five purchases.
* What is the total revenue by payment method?

---

## Requirements

* Python 3.10 or newer
* An OpenAI-compatible API key
* A SQLite database file

---

## Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

Configure either a custom OpenAI-compatible provider:

```env
PROXY_API_KEY="your-provider-api-key"
PROXY_BASE_URL="https://your-provider-base-url/v1"
OPENAI_MODEL="gpt-4o-mini"
LLM_TEMPERATURE="0"
```

Or use OpenAI directly:

```env
OPENAI_API_KEY="your-openai-api-key"
OPENAI_BASE_URL="https://api.openai.com/v1"
OPENAI_MODEL="gpt-4o-mini"
LLM_TEMPERATURE="0"
```

The application checks the `PROXY_*` variables first and falls back to the corresponding `OPENAI_*` variables.

Never commit a real `.env` file or API key.

---

## Local Quickstart

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create the local environment file:

```bash
cp .env.example .env
```

Add your provider credentials to `.env`, then run the application:

```bash
python app.py
```

Open the Gradio URL displayed in the terminal.

Upload a SQLite database and ask a question such as:

> Show the top five customers by total revenue.

---

## Query Validation

Before execution, generated SQL passes through a lightweight validation layer.

The prototype:

* rejects multiple SQL statements
* parses SQL using the SQLite dialect
* accepts only a single `SELECT` statement
* rejects configured mutation and administration keywords
* blocks access to internal SQLite tables
* executes the query through a read-only SQLite connection
* adds a result limit when the generated query does not contain one

These checks reduce obvious risks but should not be treated as a complete production security model.

Production systems should also consider:

* database-level permissions
* isolated execution environments
* query cost and timeout enforcement
* schema and column allowlists
* tenant-aware access control
* audit logging
* sensitive-data policies
* adversarial and regression testing

---

## Deploying to Hugging Face Spaces

Create a new Hugging Face Space with the following settings:

* SDK: Gradio
* Python: 3.11
* Application file: `app.py`
* Hardware: CPU Basic

Add the project files to the Space repository.

In the Space settings, configure the required secrets:

```text
PROXY_API_KEY
PROXY_BASE_URL
OPENAI_MODEL
```

Alternatively, configure:

```text
OPENAI_API_KEY
OPENAI_BASE_URL
OPENAI_MODEL
```

Do not commit API keys to the repository.

Uploaded databases are temporary in the default Hugging Face Spaces environment.

---

## Limitations

This prototype does not include:

* user authentication or authorization
* tenant isolation
* database permission management
* comprehensive SQL policy enforcement
* query planning or cost estimation
* automated SQL repair
* evaluation and regression datasets
* production observability
* persistent audit records
* PostgreSQL support
* a dedicated backend API

These concerns are addressed more systematically in the successor project.

---

## Successor Project

The actively developed version includes a FastAPI backend and stronger engineering boundaries around SQL generation, validation, verification, bounded repair, evaluation, observability, and failure handling.

➡️ [NL2SQL Copilot](https://github.com/melika-kheirieh/nl2sql-copilot)
