# Email and Date Extractor from Server Logs

This project processes server log files to extract email addresses and their corresponding dates, transform and format the data, and store it in both a MongoDB and an SQL database for further analysis and historical tracking.

---

## Features

1. **Email and Date Extraction**:
   - Reads server logs.
   - Extracts email addresses and associated dates.

2. **Data Transformation**:
   - Formats dates to the standard `YYYY-MM-DD HH:MM:SS` format.
   - Structures data for database storage.

3. **Data Storage**:
   - Saves processed data into a MongoDB collection named `user_history`.
   - Transfers data to an SQL database with the same table name and relevant constraints.

4. **Data Analysis**:
   - Provides SQL queries for data analysis.

---

## Project Workflow

### Task 1: Extract Email Addresses and Dates
- Extract email addresses using regex patterns.
- Match dates associated with each email.

### Task 2: Data Transformation
- Parse and format extracted dates.
- Prepare structured data for insertion into databases.

### Task 3: Save Data to MongoDB
- Store extracted and transformed data in a MongoDB collection.

### Task 4: Save Data to SQL Database
- Fetch data from MongoDB.
- Insert into a relational database (e.g., SQLite) with primary key constraints.

### Task 5: Run Queries on the SQL Database
- Execute predefined SQL queries to analyze stored data.

---

## Prerequisites

- Python 3.8+
- MongoDB
- SQLite or any SQL-compatible database
- Required Python libraries:
  - `pymongo`
  - `sqlite3`
  - `re`
  - `datetime`



---



## Code Overview

### Main Functions

#### 1. Extract Emails and Dates
```python
email_pattern = r"From: ([\w\.-]+@[\w\.-]+\.\w+)"
date_pattern = r"(Mon|Tue|Wed|Thu|Fri|Sat|Sun) [A-Za-z]+ [0-9]{1,2} [0-9]{2}:[0-9]{2}:[0-9]{2} [0-9]{4}"
```
- Extracts emails and dates from log files.
- Formats dates using:
  ```python
  formatted_date = datetime.strptime(date_str, "%a %b %d %H:%M:%S %Y").strftime("%Y-%m-%d %H:%M:%S")
  ```

#### 2. Save to MongoDB
```python
client = pymongo.MongoClient("mongodb://localhost:27017/")
db = client["log_data"]
collection = db["user_history"]
collection.insert_many(data)
```

#### 3. Save to SQL Database
```python
conn = sqlite3.connect("user_history.db")
cursor = conn.cursor()
cursor.execute("CREATE TABLE IF NOT EXISTS user_history (id INTEGER PRIMARY KEY AUTOINCREMENT, email TEXT, date DATETIME)")
```

#### 4. Run SQL Queries


---

1. **List all unique email addresses**:
   ```sql
   SELECT DISTINCT email FROM user_history;
   ```

2. **Count emails received per day**:
   ```sql
   SELECT DATE(date) AS day, COUNT(*) AS count FROM user_history GROUP BY day;
   ```

3. **Find the first and last email date for each email address**:
   ```sql
   SELECT email, MIN(date) AS first_date, MAX(date) AS last_date FROM user_history GROUP BY email;
   ```

4. **Count total emails from each domain**:
   ```sql
   SELECT SUBSTR(email, INSTR(email, '@') + 1) AS domain, COUNT(*) AS total FROM user_history GROUP BY domain;
   ```

---

