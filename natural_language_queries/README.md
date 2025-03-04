# Baseball Analytics with DuckDB and LLMs

This project combines baseball statistics, DuckDB, and Large Language Models to create a natural language interface for querying baseball data.

## Overview

This system allows you to:
1. Fetch and store historical baseball data using `pybaseball`
2. Save the data efficiently in a DuckDB database
3. Query the database using natural language through LLMs

## Setup


```bash 
conda env create -f environment.yaml
conda activate baseball-analytics

```
### Verify:
```
python -c "import pybaseball; import duckdb; print('Setup successful!')"
```


## Data Collection and Storage

```
python
import duckdb
import pandas as pd
from pybaseball import statcast, batting_stats_range, pitching_stats_range
```

## Initialize DuckDB
```
con = duckdb.connect('baseball_stats.db')
```
## Example: Collecting and storing a season of Statcast data

```
def store_statcast_data(year):
    # Fetch data month by month to manage memory
    for month in range(1, 13):
    start_date = f"{year}-{month:02d}-01"
    end_date = f"{year}-{month:02d}-28" # Simplified for example

df = statcast(start_dt=start_date, end_dt=end_date)

# Store in DuckDB
con.execute("""
CREATE TABLE IF NOT EXISTS statcast_data AS
SELECT FROM df WHERE 1=0
""")
con.execute("INSERT INTO statcast_data SELECT FROM df")
```

## Example: Storing batting statistics


```
def store_batting_stats(year):
    df = batting_stats_range(f"{year}-01-01", f"{year}-12-31")
    con.execute("""
    CREATE TABLE IF NOT EXISTS batting_stats AS
    SELECT FROM df WHERE 1=0
    """)
    con.execute("INSERT INTO batting_stats SELECT FROM df")
```

## Natural Language Queries

Using LLMs, we can translate natural language questions into SQL queries:

* The LLM would generate SQL based on the question
  
### Example questions:
  *  "Who hit the most home runs in 2023?"
  *  
  *  "What was the average exit velocity for Pete Alonso in 2023?"
  *  
  *  "Show me all pitchers who threw over 95mph on average in 2023"

``` python
def natural_language_query(question):
    # Example prompt for the LLM
        prompt = f"""
        Given the following tables in our baseball database:
        statcast_data (pitch-by-pitch data)
        batting_stats (season batting statistics)
        Convert this question into a SQL query:
        "{question}"
        """
```

## Putting It All Together




