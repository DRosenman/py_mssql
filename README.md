# polars_mssql

`polars_mssql` is a Python package designed to simplify working with Microsoft SQL Server databases using the high-performance `polars` DataFrame library. It provides an intuitive and efficient interface for running SQL queries, reading tables, and writing data to SQL Server.

## Features

- **Seamless SQL Server Integration**: Easily connect to SQL Server with options for Windows Authentication or SQL Authentication.
- **Query Execution**: Execute SQL queries and retrieve results as `polars.DataFrame` objects.
- **Table Operations**: Read and write tables with flexibility and performance.
- **Context Management**: Supports Python's context manager for automatic connection handling.
- **Customizable Options**: Configure schema inference, batch sizes, and execution options.

## Installation

Install the package using pip:

```bash
pip install polars_mssql
```

Ensure the following dependencies are installed:

- `polars` for high-performance DataFrame operations.
- `sqlalchemy` for database connectivity.
- An appropriate ODBC driver for SQL Server (e.g., ODBC Driver 17 or 18).

## Usage

Here is an example of how to use `polars_mssql` to connect to SQL Server and perform various operations:

### 1. Connecting to SQL Server

```python
from polars_mssql import Connection

# Initialize a connection
conn = Connection(
    database="my_database",
    server="my_server",
    driver = 'ODBC Driver 17 for SQL Server'
)
```

### 2. Running Queries

#### Execute a SQL Query and Get Results as a DataFrame

```python
query = "SELECT * FROM my_table WHERE col1 = 'a'"
df = conn.read_query(query)
```

#### Read an Entire Table

```python
df = conn.read_table("my_table")
```

### 3. Writing Data to SQL Server

#### Write a Polars DataFrame to a Table

```python
import polars as pl

# Example DataFrame
data = pl.DataFrame({"col1": [1, 2, 3], "col2": ["a", "b", "c"]})
conn.write_table(data, name="my_table", if_exists="replace")
```

### 4. Using Context Management

```python
with Connection(database="my_database", server="my_server") as conn:
    df = conn.read_query("SELECT * FROM my_table")
    print(df)
```

### 5. Closing the Connection

```python
conn.close()
```

## API Reference

### `Connection` Class

#### Constructor
```python
Connection(database: Optional[str] = None, server: Optional[str] = None, driver: Optional[str] = None, username: Optional[str] = None, password: Optional[str] = None)
```

#### Methods

- **`read_query(query: str, ...) -> pl.DataFrame`**: Execute a query and return results as a Polars DataFrame.
- **`read_table(name: str) -> pl.DataFrame`**: Read all rows from a table.
- **`write_table(df: pl.DataFrame, name: str, ...)`**: Write a Polars DataFrame to a table.
- **`close()`**: Close the database connection.
- **Context Management (`__enter__` and `__exit__`)**: Automatically manage connections.

## Requirements

- Python 3.8 or higher
- ODBC Driver for SQL Server (17 or 18 recommended)

### Installing the ODBC Driver

#### Windows
Download and install the ODBC Driver from [Microsoft's website](https://learn.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server).

#### macOS
Install via Homebrew:
```bash
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install --no-sandbox msodbcsql18
```

#### Linux
Install using the following commands:
```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
sudo apt-get update
sudo apt-get install -y mssql-tools unixodbc-dev
```

## Contributing

Contributions are welcome! If you encounter issues or have feature requests, please open an issue or submit a pull request on [GitHub](https://github.com/drosenman/polars_mssql).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

This package integrates the efficiency of polars with the versatility of SQL Server, inspired by real-world data engineering needs. As a data engineer, I often need to pull data from SQL Server into polars and export data from polars back to SQL Server. I created this package to streamline these workflows and make the process more efficient.
