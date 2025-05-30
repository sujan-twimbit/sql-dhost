# sql-dhost

**sql-dhost** is a Python utility that integrates SQL database querying with powerful LLMs (like OpenAI, Claude, and Perplexity) to enable natural language to SQL query conversion. It's particularly useful for users who want to interact with databases using plain English.

## 🚀 Features

- 🔌 Easy connection to PostgreSQL databases
- 🤖 Convert natural language questions into SQL using OpenAI, Claude, or Perplexity
- 🧠 Schema-aware LLM prompts
- 🔍 Execute queries and retrieve results programmatically
- ⚙️ Modular design for easy extension

---

## 📦 Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/sujanMidatani7/sql-dhost.git
cd sql-dhost
pip install uv
uv init
uv venv
uv add -r requirements.txt
```

---

## 🛠️ Usage

### 1. **Basic Setup**

```python
from dhost import PSQLUtil

# PostgreSQL connection string
conn_str = "db_connection_string"

# Initialize PSQLUtil with API keys
psql = PSQLUtil(
    db_connection=conn_str,
    openai_api_key="your-openai-key",
    claude_api_key="your-claude-key",
    perplexity_api_key="your-perplexity-key"
)
```

---

### 2. **Set Schema**

Before generating queries, provide your database schema:

```python
schema = """
Table: users
Columns: id, name, email

Table: orders
Columns: id, user_id, amount, date
"""

psql.set_schema(schema)
```

---

### 3. **Execute Raw SQL Queries**

```python
query = "SELECT * FROM users WHERE id = %s"
results = psql.execute_query(query, params=(1,))
print(results)
```

---

### 4. **Generate SQL from Natural Language**

```python
response = psql.generate_sql_query(
    question="List all users who placed an order above $1000",
    provider="openai",
    llm="gpt-4o",
    max_tokens=500
)

print(response['sql'])
```

---

### 5. **Generate SQL and Execute It**

```python
for output in psql.generate_sql_query_and_execute(
    question="Get all users who made orders in January",
    provider="claude",
    llm="claude-3-opus"
):
    print(output)
```

---

## 🧪 Testing

To run test scripts:

```bash
python test.py
```

Ensure your `.env` or connection config is properly set up before testing.

---

## 🔧 API Reference

### `PSQLUtil.__init__(...)`
Initializes the utility with DB and optional LLM API keys.

### `set_schema(schema: str)`
Sets the database schema and updates system prompt for the LLM.

### `execute_query(query: str, params: tuple = None)`
Executes a SQL query and returns results.

### `update_system_prompt(new_prompt: str)`
Manually update the LLM system prompt.

### `get_system_prompt() -> str`
Retrieves the current system prompt.

### `generate_sql_query(question: str, provider="openai", llm=None, max_tokens=1000)`
Returns SQL and explanation generated by selected LLM.

### `generate_sql_query_and_execute(...)`
Generates SQL using LLM and streams execution results.

---

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first to discuss.

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.