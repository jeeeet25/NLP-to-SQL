# Speak SQL: Harnessing OpenAI for Effortless Natural Language Querying

## Overview
Speak SQL is a groundbreaking project aimed at simplifying the process of converting natural language queries into SQL commands. Whether you're a non-technical stakeholder or a seasoned SQL expert, our solution bridges the gap between human language and database operations, making data querying more intuitive and accessible.

---

## Features

- **Natural Language Processing**: Transform your everyday language queries into actionable SQL commands.
- **Efficient Database Integration**: Seamless integration with SQL databases using SQLAlchemy.
- **User-friendly Interface**: Aiming to develop a user interface for easy interaction and data visualization.

---

## Project Architecture

 ![image](https://github.com/jeeeet25/NLP-to-SQL/assets/40267283/f7ac79d6-1497-4884-8bf5-5f1455f8d766)

1. **CSV Import**: Import data from a CSV file into a Pandas DataFrame.
2. **SQL Database Setup**: Create a temporary in-memory database using SQLAlchemy.
3. **API Integration**: Utilize OpenAI's `text-davinci-002` model for prompt engineering.
4. **Query Execution**: Generate SQL queries based on user prompts and execute them against the database.

---

## Usage

### SQL Database Set-up

```python
from sqlalchemy import create_engine
from sqlalchemy import text

temp_db = create_engine('sqlite:///:memory:', echo=True)
data = df.to_sql(name='Sales', con=temp_db)

# Connecting to SQL Database
with temp_db.connect() as conn:
    result = conn.execute(text("SELECT ORDERNUMBER, SALES FROM Sales ORDER BY SALES DESC LIMIT 1"))

result.all()
```

### Generating SQL Queries with OpenAI

```python
# Combine the results in one function
def combine_prompts(df, query_prompt):
    definition = create_table_definition_prompt(df)
    query_init_string = f"### A query to answer: {query_prompt}\nSELECT"
    return definition + query_init_string

# Handle API response
response = openai.Completion.create(
  model="code-davinci-002",
  prompt=combine_prompts(df, nlp_text),
  # ... (other parameters)
)

def handle_response(response):
    query = response["choices"][0]["text"]
    if query.startswith(" "):
        query = "SELECT" + query
    return query

# Pass it to the database and get results
with temp_db.connect() as conn:
    result = conn.execute(text(handle_response(response)))

result.all()
```

---

## Future Enhancements

- **Front-end Development**: Designing a user-friendly interface with key components for input, output, and analysis recommendations.
- **Analytics Platform**: A centralized hub for data analytics, integrated with advanced visualization tools.

---

## Conclusion

Speak SQL is more than just a project; it's a vision for the future of data querying. By combining the power of OpenAI's text-davinci-002 model with innovative architecture, we're paving the way for a new era of database interaction. Join us on this exciting journey and experience the magic of language seamlessly transforming into structured database commands.

---

## Connect with Us

For inquiries and collaboration opportunities, feel free to connect with me on [LinkedIn](https://linkedin.com/in/jeetpattel). Let's build a community where language and technology converge to create shared experiences!
