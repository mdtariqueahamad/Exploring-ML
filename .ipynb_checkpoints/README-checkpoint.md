# 🧠 Machine Learning Engineering: Data Gathering & Ingestion Masterclass

[![Python Version](https://img.shields.io/badge/Python-3.9%20%7C%203.10%20%7C%203.11-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![MySQL](https://img.shields.io/badge/MySQL-Connector-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-Yes-brightgreen.svg?style=for-the-badge)](#)

> **80% of Machine Learning engineering is won or lost in data pipelines.**  
> A production-grade, highly hands-on laboratory covering the complete lifecycle of **data gathering, parsing, ingestion, memory optimization, and error handling** across diverse data sources: Delimited Text (CSV/TSV), Semi-Structured Data (JSON/REST APIs), and Relational Databases (SQL).

---

## 📑 Interactive Table of Contents

- [📐 Ingestion Architecture & Pipeline](#pipeline)
- [📂 Repository Directory Structure](#structure)
- [📊 Datasets Matrix & Specifications](#datasets)
- [🔬 Module 1: Advanced CSV & TSV Ingestion](#module-csv)
  - [⚡ Parameter Quick-Reference Table](#param-table)
  - [🔍 Deep-Dive Parameter Cards](#param-cards)
- [🧩 Module 2: JSON & Relational SQL Ingestion](#module-json-sql)
  - [JSON: Local & Remote Payloads](#json-ingestion)
  - [SQL: Relational Database Extraction](#sql-ingestion)
- [🌐 Module 3: Web REST API Harvesting & Pagination](#module-api)
- [🚀 Quickstart & Environment Setup](#quickstart)
- [💾 Memory Optimization Playbook](#memory-playbook)
- [🛠️ Troubleshooting & FAQ](#faq)
- [✅ Learning Roadmap & Checklist](#roadmap)

---

<a id="pipeline"></a>
## 📐 Ingestion Architecture & Pipeline

Every real-world ML lifecycle begins with disparate, imperfect data formats. This repository models how to transform messy external inputs into clean, memory-efficient Pandas DataFrames:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        RAW DATA SOURCES                                │
├───────────────┬────────────────────────┬───────────────────────────────┤
│ Delimited     │ Hierarchical           │ Live & Relational             │
│ • Local CSV   │ • Nested JSON Records  │ • Paginated REST APIs         │
│ • Remote CSV  │ • REST API Endpoints   │ • MySQL Enterprise DB         │
│ • Tab-Sep TSV │ • Complex Key-Values   │ • Cloud Data Stores           │
└───────┬───────┴───────────┬────────────┴───────────────┬───────────────┘
        │                   │                            │
        ▼                   ▼                            ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   INGESTION & PARSING ENGINE                           │
├────────────────────────────────────────────────────────────────────────┤
│ • Encoding Resolver (UTF-8, Latin-1, ISO-8859)                         │
│ • Chunked Generator Streaming (OOM prevention on multi-GB files)       │
│ • Custom Converters & In-flight Datetime Parse                         │
│ • Dtype Downcasting & Custom Null-value Mapping                        │
│ • API Pagination Loop + In-memory Data Normalization                   │
└────────────────────────────────────────────────┬───────────────────────┘
                                                 │
                                                 ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   TARGET MACHINE LEARNING DATAFRAME                    │
│        (Ready for Exploratory Data Analysis, Cleaning & Modeling)      │
└────────────────────────────────────────────────────────────────────────┘
```

---

<a id="structure"></a>
## 📂 Repository Directory Structure

```text
Machine Learning/
│
├── CSV/                                   # Delimited Files Ingestion Lab
│   ├── working_with_csv.ipynb             # 16-step Master notebook for pd.read_csv
│   ├── aug_train.csv                      # HR analytics dataset (19,158 rows × 14 cols)
│   ├── IPL Matches 2008-2020.csv          # Sports analytics & dates (816 rows × 17 cols)
│   ├── movie_titles_metadata.tsv          # Tab-separated Cornell movie dialogs (616 rows)
│   ├── zomato.csv                         # Restaurant reviews with non-UTF8 encoding (9,551 rows)
│   ├── student_placement.csv              # Feature & target classification data (500 rows)
│   └── test.csv                           # Offset header test file
│
├── JSON & SQL/                            # Semi-structured & Relational Lab
│   ├── working_with_json_and_sql.ipynb    # JSON API fetch & MySQL connectivity guide
│   └── train.json                         # Recipe ingredients dataset (39,774 recipe records)
│
├── Fetching from API/                     # Programmatic Ingestion & Web Harvesting
│   ├── API.ipynb                          # CricAPI offset pagination crawler
│   └── cric_ply.csv                       # Harvested international cricketers (1,525 players)
│
├── .gitignore                             # Clean repo governance (.env, .DS_Store)
└── README.md                              # Repository Documentation & Study Handbook
```

---

<a id="datasets"></a>
## 📊 Datasets Matrix & Specifications

| Dataset | Location | Rows | Cols | Size | Primary Engineering Challenge |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **HR Analytics (`aug_train.csv`)** | `CSV/` | 19,158 | 14 | ~1.9 MB | Index assignment, type downcasting, chunk iteration |
| **IPL Matches (`IPL Matches 2008-2020.csv`)** | `CSV/` | 816 | 17 | ~147 KB | Datetime parsing (`parse_dates`), custom converter functions |
| **Cornell Movies (`movie_titles_metadata.tsv`)** | `CSV/` | 616 | 6 | ~41 KB | Missing headers, custom `sep='\t'`, explicit column naming |
| **Zomato Global (`zomato.csv`)** | `CSV/` | 9,551 | 21 | ~2.2 MB | Multi-language text decoding (`encoding='latin-1'`) |
| **Student Placement (`student_placement.csv`)** | `CSV/` | 500 | 4 | ~7.7 KB | Lightweight numeric sanity testing & schema checks |
| **Recipe Ingredients (`train.json`)** | `JSON & SQL/` | 39,774 | 3 | ~12.1 MB | Nested JSON arrays (`ingredients`), JSON array of objects |
| **CricAPI Harvest (`cric_ply.csv`)** | `Fetching from API/` | 1,525 | 3 | ~44 KB | Offset-based API harvesting and normalized schema export |

---

<a id="module-csv"></a>
## 🔬 Module 1: Advanced CSV & TSV Ingestion

The notebook [`CSV/working_with_csv.ipynb`](CSV/working_with_csv.ipynb) demonstrates practical solutions for loading imperfect delimited files using `pandas.read_csv()`.

<a id="param-table"></a>
### ⚡ Parameter Quick-Reference Table

| # | Parameter | Purpose | Typical Scenario |
| :-: | :--- | :--- | :--- |
| **1** | `filepath_or_buffer` | Local path, URL, or buffer | Loading from disk or remote raw GitHub links |
| **2** | `sep` / `delimiter` | Field separator character | Handling TSV (`\t`), pipes (`\|`), or semicolons (`;`) |
| **3** | `names` | List of column names | Replacing missing or faulty headers |
| **4** | `index_col` | Primary key column | Setting record IDs as DataFrame index |
| **5** | `header` | Row index of column labels | Skipping dirty metadata lines before headers |
| **6** | `usecols` | Subset of columns to load | Reducing memory footprint by ignoring unused columns |
| **7** | `squeeze` | Return Series if 1 column | Extracting single feature targets (legacy / `.squeeze()`) |
| **8** | `nrows` / `skiprows` | Slice rows on ingestion | Reading quick previews or skipping corrupt rows |
| **9** | `encoding` | Text encoding standard | Resolving `UnicodeDecodeError` (`latin-1`, `cp1252`) |
| **10** | `on_bad_lines` | Error handling for bad rows | Skipping rows with uneven delimiter counts |
| **11** | `dtype` | Explicit type assignment | Preventing automatic upcasting to save memory |
| **12** | `parse_dates` | Convert text to `datetime64` | Ingesting dates directly without post-hoc parsing |
| **13** | `converters` | In-flight transform functions | Cleaning strings, abbreviations, or values on load |
| **14** | `na_values` | Custom missing value markers | Converting values like `'?'`, `'None'`, `'Male'` to `NaN` |
| **15** | `chunksize` | Chunked generator stream | Loading multi-GB datasets without Out-of-Memory (OOM) |

---

<a id="param-cards"></a>
### 🔍 Deep-Dive Parameter Cards

Click on any card to view the explanation, syntax, and sample output:

<details>
<summary><b>1. Opening Remote CSV Files via HTTP/HTTPS (Click to expand)</b></summary>
<br>

**Why it matters:**  
Instead of downloading files manually to local disk, stream data directly from raw endpoints or cloud object stores (S3, GCS, GitHub raw) using standard HTTP requests.

```python
import pandas as pd
import requests
from io import StringIO

url = "https://raw.githubusercontent.com/cs109/2014_data/master/countries.csv"
headers = {"User-Agent": "Mozilla/5.0"}
response = requests.get(url, headers=headers)

# Wrap text response into an in-memory buffer
data_buffer = StringIO(response.text)
df = pd.read_csv(data_buffer)
print(df.head(3))
```

*Key tip:* Always provide a custom `User-Agent` header to prevent 403 Forbidden errors when scraping raw endpoints.
</details>

<details>
<summary><b>2. Handling Custom Separators & Missing Headers (<code>sep</code> &amp; <code>names</code>)</b></summary>
<br>

**Why it matters:**  
Many real-world files are not comma-separated (e.g., TSV files, pipe-separated logs). Furthermore, metadata files often omit a header row.

```python
# movie_titles_metadata.tsv is tab-separated and lacks column headers
df = pd.read_csv(
    'CSV/movie_titles_metadata.tsv',
    sep='\t',
    names=['sno', 'name', 'release_year', 'rating', 'votes', 'genres']
)
print(df[['name', 'release_year', 'rating']].head(3))
```

```text
                         name release_year  rating
0  10 things i hate about you         1999     6.9
1  1492: conquest of paradise         1992     6.2
2                  15 minutes         2001     6.1
```
</details>

<details>
<summary><b>3. Primary Key Indexing (<code>index_col</code>)</b></summary>
<br>

**Why it matters:**  
By default, Pandas introduces an extra `0, 1, 2...` integer RangeIndex. Specifying `index_col` prevents redundant index allocation and enables $O(1)$ lookup times.

```python
# Set enrollee_id as the primary DataFrame index
df = pd.read_csv('CSV/aug_train.csv', index_col='enrollee_id')
print(df.index.name)  # 'enrollee_id'
```
</details>

<details>
<summary><b>4. Skipping Offset Metadata & Dirty Headers (<code>header</code> &amp; <code>skiprows</code>)</b></summary>
<br>

**Why it matters:**  
Business reports exported from legacy software often contain company headers or summary disclaimers on rows 0 to 4 before the actual table starts.

```python
# If the real header row is located on line 1 (0-indexed line 2):
df = pd.read_csv('CSV/test.csv', header=1)

# Or skip arbitrary non-data rows:
df = pd.read_csv('CSV/aug_train.csv', skiprows=[0, 5, 20])
```
</details>

<details>
<summary><b>5. Selective Column Ingestion for RAM Optimization (<code>usecols</code>)</b></summary>
<br>

**Why it matters:**  
If a dataset contains 100 columns but your model only needs 3 features, reading all 100 wastes system memory and increases disk I/O latency.

```python
df = pd.read_csv(
    'CSV/aug_train.csv',
    usecols=['enrollee_id', 'gender', 'education_level']
)
print(df.columns.tolist())
# ['enrollee_id', 'gender', 'education_level']
```
</details>

<details>
<summary><b>6. Resolving Character Encoding Issues (<code>encoding</code>)</b></summary>
<br>

**Why it matters:**  
Pandas defaults to `utf-8`. Files containing international characters, accents, or currency symbols often crash with `UnicodeDecodeError: 'utf-8' codec can't decode byte...`.

```python
# The Zomato dataset contains international restaurant names:
df = pd.read_csv('CSV/zomato.csv', encoding='latin-1')
# Alternative encodings: 'cp1252', 'iso-8859-1', 'utf-16'
```
</details>

<details>
<summary><b>7. Handling Corrupted & Malformed Rows (<code>on_bad_lines</code>)</b></summary>
<br>

**Why it matters:**  
Rows with unescaped quotes or stray commas have more fields than expected, throwing `ParserError: Error tokenizing data`.

```python
# Modern Pandas (>=1.3.0):
df = pd.read_csv('corrupted_dataset.csv', on_bad_lines='skip')

# Legacy Pandas (<1.3.0):
# df = pd.read_csv('corrupted_dataset.csv', error_bad_lines=False)
```
</details>

<details>
<summary><b>8. In-flight Type Downcasting (<code>dtype</code>)</b></summary>
<br>

**Why it matters:**  
Pandas defaults integers and floats to 64-bit precision (`int64`, `float64`), consuming 8 bytes per cell. Specifying exact narrower dtypes can slash RAM usage by up to 75%.

```python
df = pd.read_csv('CSV/aug_train.csv', dtype={'target': 'int8'})
print(df['target'].dtype)  # int8 instead of default float64
```
</details>

<details>
<summary><b>9. Native Datetime Parsing (<code>parse_dates</code>)</b></summary>
<br>

**Why it matters:**  
Parsing dates during load avoids a second pass with `pd.to_datetime()` later in your script.

```python
df = pd.read_csv('CSV/IPL Matches 2008-2020.csv', parse_dates=['date'])
print(df['date'].dtype)  # datetime64[ns]
print(df['date'].dt.year.head(2))
```
</details>

<details>
<summary><b>10. Ingest-Time Transformations (<code>converters</code>)</b></summary>
<br>

**Why it matters:**  
Apply clean-up logic, standardize company/team names, or strip units directly while reading lines from disk.

```python
def standardize_team(name):
    if name == "Royal Challengers Bangalore":
        return "RCB"
    return name

df = pd.read_csv(
    'CSV/IPL Matches 2008-2020.csv',
    converters={'team1': standardize_team}
)
print(df['team1'].value_counts().head(3))
```
</details>

<details>
<summary><b>11. Custom Missing Value Detection (<code>na_values</code>)</b></summary>
<br>

**Why it matters:**  
Datasets often encode nulls using non-standard strings like `'-'`, `'9999'`, `'N/A'`, or `'MISSING'`.

```python
# Treat both custom string indicators as NaN during parsing:
df = pd.read_csv('CSV/aug_train.csv', na_values=['Missing', 'Unknown'])
```
</details>

<details>
<summary><b>12. Streaming Giant Datasets via Generator Chunks (<code>chunksize</code>)</b></summary>
<br>

**Why it matters:**  
Attempting to read a 15GB file into an 8GB machine results in an OS crash (Out-Of-Memory). Using `chunksize` returns an iterable `TextFileReader` that streams records in manageable blocks.

```python
chunk_iterator = pd.read_csv('CSV/aug_train.csv', chunksize=5000)

total_records = 0
for idx, chunk in enumerate(chunk_iterator):
    print(f"Processing Chunk #{idx + 1} with shape: {chunk.shape}")
    total_records += len(chunk)

print(f"Total rows processed safely: {total_records}")
```

```text
Processing Chunk #1 with shape: (5000, 14)
Processing Chunk #2 with shape: (5000, 14)
Processing Chunk #3 with shape: (5000, 14)
Processing Chunk #4 with shape: (4158, 14)
Total rows processed safely: 19158
```
</details>

---

<a id="module-json-sql"></a>
## 🧩 Module 2: JSON & Relational SQL Ingestion

The notebook [`JSON & SQL/working_with_json_and_sql.ipynb`](JSON%20&%20SQL/working_with_json_and_sql.ipynb) explores handling nested structures and database queries.

<a id="json-ingestion"></a>
### 1. JSON Semi-Structured Ingestion

JSON is the standard format for web data, document databases (MongoDB), and microservices.

```python
import pandas as pd

# 1. Loading local hierarchical JSON (Recipes dataset)
# train.json contains 39,774 recipes with lists of ingredients
df_recipes = pd.read_json('JSON & SQL/train.json')
print(df_recipes[['cuisine', 'ingredients']].head(2))

# 2. Loading directly from a live JSON REST endpoint
url = 'https://api.exchangerate-api.com/v4/latest/INR'
df_rates = pd.read_json(url)
print(df_rates.head(2))
```

<details>
<summary><b>💡 Pro-Tip: Normalizing deeply nested JSON</b></summary>
<br>

When a JSON payload contains nested child dictionaries or lists, use `pd.json_normalize()`:

```python
raw_payload = [
    {"user": "Alice", "meta": {"city": "New York", "active": True}},
    {"user": "Bob", "meta": {"city": "London", "active": False}}
]

df_normalized = pd.json_normalize(raw_payload)
# Resulting columns: 'user', 'meta.city', 'meta.active'
```
</details>

---

<a id="sql-ingestion"></a>
### 2. SQL Relational Database Ingestion

Instead of exporting CSV dumps from production databases, connect directly with database drivers like `mysql-connector-python` or `SQLAlchemy`.

```python
import pandas as pd
import mysql.connector

# Establish secure database connection
conn = mysql.connector.connect(
    host='localhost',
    user='root',
    password='',
    database='world'
)

# Execute SQL query directly into a Pandas DataFrame
query = """
    SELECT CountryCode, Language, IsOfficial, Percentage
    FROM countrylanguage
    WHERE Percentage > 50.0
    ORDER BY Percentage DESC;
"""
df_languages = pd.read_sql_query(query, conn)

# Close connection
conn.close()
```

---

<a id="module-api"></a>
## 🌐 Module 3: Web REST API Harvesting & Pagination

The notebook [`Fetching from API/API.ipynb`](Fetching%20from%20API/API.ipynb) demonstrates building a resilient data crawler that paginates through a live third-party REST API.

### Key Architecture: The Pagination Loop

Many REST APIs limit response payloads (e.g., 25 or 50 items per call). To assemble a complete training dataset, you must paginate across offsets or page numbers:

```python
import os
import requests
import pandas as pd
from dotenv import load_dotenv

# 1. Secure credential management via .env
load_dotenv()
API_KEY = os.getenv("CRICAPI_KEY")

df_master = pd.DataFrame()

# 2. Harvest 60 batches of player profiles
for offset in range(60):
    url = f"https://api.cricapi.com/v1/players?apikey={API_KEY}&offset={offset}"
    response = requests.get(url)
    
    if response.status_code == 200:
        data = response.json().get('data', [])
        if not data:
            print(f"Reached end of data at offset {offset}")
            break
            
        # Drop ID or metadata columns if needed and concatenate
        batch_df = pd.DataFrame(data).iloc[:, 1:]
        df_master = pd.concat([df_master, batch_df], ignore_index=True)
    else:
        print(f"Error {response.status_code} at offset {offset}")
        break

# 3. Export harvested dataset to CSV
df_master.to_csv('Fetching from API/cric_ply.csv', index=False)
print(f"Successfully harvested {len(df_master)} records.")
```

---

<a id="quickstart"></a>
## 🚀 Quickstart & Environment Setup

Get this repository up and running in under 2 minutes.

### 1. Clone the Repository

```bash
git clone https://github.com/mdtariqueahamad/Exploring-ML.git
cd Exploring-ML
```

### 2. Create and Activate Virtual Environment

<details open>
<summary><b>Option A: Using <code>uv</code> (Fastest)</b></summary>
<br>

```bash
uv venv
source .venv/bin/activate
uv pip install pandas numpy requests python-dotenv mysql-connector-python jupyterlab
```
</details>

<details>
<summary><b>Option B: Standard <code>python3 -m venv</code></b></summary>
<br>

```bash
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -r requirements.txt  # Or install individual packages
```
</details>

### 3. Configure API Credentials (Optional)

If running the CricAPI harvester notebook:
```bash
# Create your local .env file
echo "CRICAPI_KEY=your_actual_api_key_here" > .env
```

### 4. Launch Jupyter Notebooks

```bash
jupyter lab
```

---

<a id="memory-playbook"></a>
## 💾 Memory Optimization Playbook

When loading large datasets into Pandas, apply this checklist to avoid excessive RAM usage:

| Problem | Root Cause | Optimization Solution | Memory Impact |
| :--- | :--- | :--- | :---: |
| **All Columns Loaded** | Default `pd.read_csv()` loads every feature | Use `usecols=['colA', 'colB']` | 📉 **40% - 80% RAM saved** |
| **Oversized Integers** | `int64` assigned to small values (e.g. 0-100) | Set `dtype={'age': 'uint8'}` | 📉 **87% reduction** per column |
| **High Cardinality Strings** | `object` dtype storing repetitive string pointers | Convert to `dtype={'cat_col': 'category'}` | 📉 **50% - 90% reduction** |
| **Out-Of-Memory (OOM)** | File size exceeds available hardware RAM | Stream records using `chunksize=N` | 🚀 **Predictable, constant RAM** |

---

<a id="faq"></a>
## 🛠️ Troubleshooting & FAQ

<details>
<summary><b>Q1: Why do I get <code>UnicodeDecodeError: 'utf-8' codec can't decode byte 0xed</code>?</b></summary>
<br>

**Solution:** The file was saved using a different character encoding (frequently Western European Latin-1 or Windows CP1252). Pass `encoding='latin-1'` or `encoding='iso-8859-1'` to `pd.read_csv()`.
</details>

<details>
<summary><b>Q2: What replaced <code>error_bad_lines=False</code> in newer Pandas versions?</b></summary>
<br>

**Solution:** In Pandas 1.3.0 and newer, `error_bad_lines` is deprecated. Use `on_bad_lines='skip'` to ignore rows with extra fields, or `on_bad_lines='warn'` to log warnings.
</details>

<details>
<summary><b>Q3: How do I handle date columns with non-standard formats?</b></summary>
<br>

**Solution:** Pass `parse_dates=['date_col']`. If the format is unconventional (e.g., `DD/MM/YYYY`), combine it with `date_format='%d/%m/%Y'` for optimal parsing speed.
</details>

<details>
<summary><b>Q4: How do I read multiple CSV files from a directory into one DataFrame?</b></summary>
<br>

**Solution:**
```python
import glob
import pandas as pd

all_files = glob.glob("CSV/*.csv")
df_combined = pd.concat((pd.read_csv(f) for f in all_files), ignore_index=True)
```
</details>

---

<a id="roadmap"></a>
## ✅ Learning Roadmap & Checklist

Track your progress as you work through each notebook:

- [x] **Delimited Files (`working_with_csv.ipynb`)**
  - [x] Reading local & remote CSV files
  - [x] Handling custom separators (`\t`, `;`)
  - [x] Setting custom indexes and column headers
  - [x] Filtering columns on read (`usecols`)
  - [x] Resolving encoding errors (`latin-1`)
  - [x] Handling corrupted rows (`on_bad_lines`)
  - [x] Specifying dtypes and date parsing
  - [x] Ingesting in chunks (`chunksize`)
- [x] **JSON & Relational SQL (`working_with_json_and_sql.ipynb`)**
  - [x] Ingesting local nested JSON arrays
  - [x] Fetching live currency exchange rates via REST endpoints
  - [x] Connecting to MySQL using `mysql.connector`
  - [x] Querying relational tables directly into Pandas
- [x] **REST API Ingestion (`API.ipynb`)**
  - [x] Managing secure credentials with `python-dotenv`
  - [x] Implementing offset-based pagination loops
  - [x] Normalizing response payloads and exporting to CSV

---

## 🤝 Contributing

Contributions, issues, and feature additions are welcome!
1. Fork this repository
2. Create a feature branch (`git checkout -b feature/NewDataSources`)
3. Commit your changes (`git commit -m 'Add XML and Parquet ingestion examples'`)
4. Push to your branch (`git push origin feature/NewDataSources`)
5. Open a Pull Request

---

## 📄 License

This repository is distributed under the MIT License. See [LICENSE](LICENSE) for details.
