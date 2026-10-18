# Text-to-SQL Agent

<p align="center">
  <strong>Ask questions in plain English. Get SQL queries and database results.</strong>
</p>

<p align="center">
  <a href="https://github.com/Suraj8619/text-to-sql-agent">GitHub Repository</a> ·
  <a href="https://github.com/Suraj8619">My GitHub Profile</a> ·
  <a href="https://www.linkedin.com/in/suraj-kumar-234173343">LinkedIn</a>
</p>

## About the Project

**Text-to-SQL Agent** is a Python project that lets users interact with a relational database using natural language instead of writing SQL manually. Built with LangChain, the agent inspects the database schema, generates SQL for a user's question, validates the query, executes it, and can retry after an error.

The project demonstrates how large language models (LLMs), database tools, and an agent workflow can work together to make database querying more accessible.

## Features

- **Natural-language querying:** Ask questions about the database in plain English.
- **SQL generation:** Convert a user's question into a SQL query.
- **Schema awareness:** Discover available tables and inspect relevant table schemas.
- **Query validation:** Check generated queries before execution.
- **Error recovery:** Retry by rewriting a query when execution errors occur.
- **Complex SQL support:** Handle use cases involving joins, aggregations, and subqueries, depending on the question and database schema.
- **Tracing and debugging:** Integrate with LangSmith to inspect agent execution and tool calls.
- **Interactive learning:** Explore the workflow through the included Jupyter notebook.

## Tech Stack

- **Language:** Python
- **LLM orchestration:** LangChain
- **Database:** SQLite
- **Database interaction:** SQLAlchemy and LangChain Community
- **Agent workflow:** LangGraph / LangChain agent tools
- **Observability:** LangSmith (optional)
- **Environment configuration:** python-dotenv
- **CLI output:** Rich
- **Tutorial:** Jupyter Notebook

> **Model configuration:** The supplied setup uses the Anthropic API. The README and environment variables below reflect that implementation.

## How It Works

The agent follows this general workflow:

1. **Receive a question** — The user enters a natural-language question, such as “Which artists generated the most sales?”
2. **Discover the database** — The agent identifies the available tables.
3. **Inspect the schema** — It retrieves relevant table definitions and sample rows to understand the data.
4. **Generate SQL** — The configured LLM creates a SQL query using the database dialect and schema context.
5. **Validate the query** — The agent checks the query before running it.
6. **Execute the query** — The SQL query runs against the SQLite database.
7. **Recover from errors** — If execution fails, the agent can use the error to revise and retry the query.
8. **Return the result** — The output is presented in a readable format.

## Demo Database

This project uses the [Chinook sample database](https://github.com/lerocha/chinook-database), which represents a digital media store. It includes data about artists, albums, tracks, customers, invoices, and related entities.

## Getting Started

### Prerequisites

- Python 3.11 or higher
- An Anthropic API key ([get one here](https://console.anthropic.com/))
- Optional: a LangSmith API key for tracing ([LangSmith](https://smith.langchain.com/))
- Git

### 1. Clone the repository

```bash
git clone https://github.com/Suraj8619/text-to-sql-agent.git
cd text-to-sql-agent
```

### 2. Download the Chinook database

Download the SQLite database into the project directory:

```bash
curl -L -o chinook.db https://github.com/lerocha/chinook-database/raw/master/ChinookDatabase/DataSources/Chinook_Sqlite.sqlite
```

If you are using Windows and do not have `curl`, download the SQLite file from the [Chinook database repository](https://github.com/lerocha/chinook-database) and save it as `chinook.db` in the project directory.

### 3. Create a virtual environment

**Using `uv`:**

```bash
uv venv --python 3.11
```

Activate it:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install the project:

```bash
uv pip install -e .
```

**Or using standard `pip`:**

```bash
python -m venv .venv
```

Activate the environment using the command for your operating system, then run:

```bash
python -m pip install -e .
```

### 4. Configure environment variables

Create a local `.env` file from the example:

```bash
# macOS / Linux
cp .env.example .env

# Windows Command Prompt
copy .env.example .env
```

Add your API key to `.env`:

```env
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

For optional LangSmith tracing, add:

```env
LANGCHAIN_TRACING_V2=true
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGCHAIN_API_KEY=your_langsmith_api_key_here
LANGCHAIN_PROJECT=text2sql-agent
```

**Security note:** Never commit `.env` or publish API keys. Keep credentials private and use `.env.example` only for placeholder values.

## Usage

### Run a query from the command line

Ask a question by passing it as an argument:

```bash
python agent.py "What are the top 5 best-selling artists?"
```

Other example questions:

```bash
python agent.py "Which employee generated the most revenue?"
python agent.py "How many customers are from Canada?"
```

The agent uses the database schema and the question to generate and execute a query.

### Explore the tutorial notebook

Launch the included notebook:

```bash
jupyter notebook tutorial.ipynb
```

The tutorial covers the agent construction process, example questions, database schema, and LangSmith tracing setup.

## LangSmith Tracing

When LangSmith is configured, it can help inspect agent runs and debug the workflow, including tool calls, generated queries, execution timing, and errors.

1. Create an account at [LangSmith](https://smith.langchain.com/).
2. Add the tracing variables to your `.env` file.
3. Run the agent and inspect the resulting traces in the LangSmith dashboard.

## Configuration

The database and agent behavior can be adjusted in `agent.py`. For example, the number of sample rows included in table information can be configured when creating the SQL database connection:

```python
db = SQLDatabase.from_uri(
    "sqlite:///chinook.db",
    sample_rows_in_table_info=3,
)
```

The default result limit can also be configured in the system prompt. Check the current implementation in `agent.py` before changing these values.

## Project Structure

```text
text-to-sql-agent/
├── agent.py                 # Core agent implementation
├── tutorial.ipynb           # Interactive Jupyter notebook
├── chinook.db               # Local sample database (not committed)
├── pyproject.toml           # Project metadata and dependencies
├── uv.lock                  # Locked dependency versions, if present
├── .env.example             # Environment variable template
├── .gitignore               # Git ignore rules
└── README.md                # Project documentation
```

Some files, such as `chinook.db` and `uv.lock`, may be absent from the repository depending on the current project setup.

## Dependencies

Project dependencies are managed through `pyproject.toml`. The original project configuration includes packages such as:

- `langchain`
- `langchain-anthropic`
- `langchain-community`
- `langgraph`
- `sqlalchemy`
- `python-dotenv`
- `rich`

Install them using the setup instructions above.

## Possible Improvements

Potential next steps for this project include:

- Add a web interface for entering questions and viewing results.
- Add support for additional database engines.
- Expand automated tests for SQL generation and error handling.
- Add query-result visualizations.
- Add safeguards and permission controls for database operations.

These are possible extensions, not claims about features already implemented.

## Author

**Suraj Kumar**  
Final-year Electronics and Communication Engineering (ECE) student at IIIT Bhagalpur, interested in software development and AI/LLM applications.

- GitHub: [@Suraj8619](https://github.com/Suraj8619)
- Project repository: [text-to-sql-agent](https://github.com/Suraj8619/text-to-sql-agent)
- LinkedIn: [Suraj Kumar](https://www.linkedin.com/in/suraj-kumar-234173343)

## Acknowledgements

- [LangChain](https://www.langchain.com/)
- [Chinook Sample Database](https://github.com/lerocha/chinook-database)
- [LangSmith](https://smith.langchain.com/)

## License

MIT

---

If you find this project useful, feel free to explore the repository and connect with me on GitHub.
