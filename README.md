# Zudio Sales Data Processing

This README file provides details about the Python script used to process, clean, and store Zudio sales data in a MySQL database.

## **Overview**
The script performs the following tasks:
1. Imports necessary dependencies.
2. Reads and preprocesses the Zudio sales dataset.
3. Cleans the data by removing duplicates and handling missing values.
4. Computes a new column `total` as the product of `Price` and `Quantity`.
5. Saves the cleaned data to a CSV file.
6. Connects to a MySQL database and uploads the cleaned data.

---

## **Prerequisites**

### Python Libraries
Ensure the following libraries are installed:
- `pandas`
- `pymysql`
- `sqlalchemy`

Install them using pip:
```bash
pip install pandas pymysql sqlalchemy
```

### MySQL Database Setup
- A MySQL database named `zudio` should exist on `localhost`.
- The user `root` with password `123456` must have access to this database.

---

## **Script Workflow**

### 1. Import Dependencies
```python
import pandas as pd
import pymysql
from sqlalchemy import create_engine
```

### 2. Read and Explore the Data
```python
# Load the dataset
df = pd.read_csv('Zudio_sales_data.csv', encoding_errors='ignore')

# Display dataset structure
df.shape
df.head()
df.describe()
df.info()
df.duplicated().sum()
df.isnull().sum()
```

### 3. Clean the Data
```python
# Drop rows with missing values
df.dropna(inplace=True)
df.isnull().sum()

# Calculate the total sales
df['total'] = df['Price'] * df['Quantity']
```

### 4. Save the Cleaned Data
```python
# Save the cleaned data to a new CSV file
df.to_csv('Zudio_clean_sales_data.csv', index=False)
```

### 5. Connect to MySQL and Upload Data
```python
# Create MySQL connection engine
engine_mysql = create_engine("mysql+pymysql://root:123456@localhost:3306/zudio")

try:
    engine_mysql
    print('connection success to mysql')
except:
    print("connection failed")

# Upload data to MySQL table 'zudio'
df.to_sql(name='zudio', con=engine_mysql, if_exists='append', index=False)
```

---

## **Expected Outputs**
1. Cleaned CSV file: `Zudio_clean_sales_data.csv`
2. MySQL table: `zudio` containing the processed data.

---

## **Error Handling**
- Ensure the `Zudio_sales_data.csv` file exists in the working directory.
- Check MySQL connection details if the connection fails.
- Verify that the `zudio` database and table structure in MySQL are compatible with the dataset.

---

## **Notes**
- Modify the database credentials (`root:123456`) as needed.
- Replace the `Price` and `Quantity` column names if they differ in the dataset.

---

## **Contact**
For further assistance, contact the data team at **data@zudio.com**.

