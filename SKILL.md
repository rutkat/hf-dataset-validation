---
name: "hf-dataset-validation"
description: "Validate Hugging Face datasets for schema, format, and data quality issues. Use when checking datasets before publishing, training, or sharing."
license: "MIT"
compatibility: "Python 3.8+, requires pandas and datasets"
metadata:
  author: "your-username"
  version: "1.0.0"
  created: "2026-04-13"
---

# Dataset Validation Skill

## Overview

This skill teaches agents how to validate Hugging Face datasets for:
- **Schema validation**: Correct columns and data types
- **Data quality**: Missing values, duplicates, outliers
- **Format compliance**: CSV, Parquet, Arrow, JSON formats
- **Size checks**: File sizes, record counts, memory footprint

Use this skill before publishing datasets or using them for training.

## Prerequisites Checklist

- [ ] Python 3.8 or higher installed
- [ ] pandas library installed (`pip install pandas`)
- [ ] datasets library installed (`pip install datasets`)
- [ ] Dataset file accessible locally

## Step-by-Step Guide

### Step 1: Install Dependencies

```bash
pip install pandas datasets numpy
```

### Step 2: Check Dataset Format

Identify what format your dataset is in:

```python
from pathlib import Path

file_path = "data/my_dataset.csv"

# Check file extension
if file_path.endswith('.csv'):
    print("Format: CSV")
elif file_path.endswith('.parquet'):
    print("Format: Parquet")
elif file_path.endswith('.json'):
    print("Format: JSON")
else:
    print("Unknown format")
```

### Step 3: Load and Inspect

Load your dataset and check basic properties:

```python
import pandas as pd

# Load CSV dataset
df = pd.read_csv("data/my_dataset.csv")

# Check shape and columns
print(f"Shape: {df.shape}")  # (rows, columns)
print(f"Columns: {list(df.columns)}")
print(f"Data types:\\n{df.dtypes}")
```

### Step 4: Validate Schema

Check that your dataset has expected columns and correct data types:

```python
# Define expected schema
expected_columns = {'text', 'label', 'split'}
actual_columns = set(df.columns)

# Check all required columns exist
missing = expected_columns - actual_columns
if missing:
    print(f"ERROR: Missing columns: {missing}")

# Check for unexpected columns
extra = actual_columns - expected_columns
if extra:
    print(f"WARNING: Extra columns: {extra}")

# Verify data types
if df['label'].dtype not in ['int64', 'object']:
    print("WARNING: label column should be integer or string")
```

### Step 5: Check Data Quality

Identify common data quality issues:

```python
# Check for missing values
missing_count = df.isna().sum()
if missing_count.any():
    print("Missing values found:")
    print(missing_count[missing_count > 0])

# Check for duplicates
duplicates = df.duplicated().sum()
if duplicates > 0:
    print(f"WARNING: {duplicates} duplicate rows found")

# Check for empty strings
for col in df.select_dtypes(include='object').columns:
    empty = (df[col].str.strip() == '').sum()
    if empty > 0:
        print(f"WARNING: Column '{col}' has {empty} empty strings")
```

### Step 6: Generate Validation Report

Use the provided validation script (see references below):

```bash
python scripts/validate_dataset.py data/my_dataset.csv
```

The report includes:
- Dataset summary (rows, columns, size)
- Data quality metrics
- Issues found and recommendations
- Remediation suggestions

## Common Issues and Solutions

### Issue: Encoding Error Reading CSV

**Problem**: `UnicodeDecodeError` when loading file

**Solution**: Try different character encodings
```python
encodings = ['utf-8', 'latin-1', 'iso-8859-1']
for encoding in encodings:
    try:
        df = pd.read_csv("file.csv", encoding=encoding)
        print(f"Success with {encoding}")
        break
    except:
        continue
```

### Issue: Memory Error with Large Files

**Problem**: `MemoryError` when loading large CSV

**Solution**: Read in chunks
```python
chunks = pd.read_csv("large_file.csv", chunksize=10000)
for i, chunk in enumerate(chunks):
    print(f"Processing chunk {i}...")
    # Process each chunk
```

### Issue: Inconsistent Column Names

**Problem**: Column names have mixed capitalization or spaces

**Solution**: Standardize column names
```python
df.columns = df.columns.str.lower().str.replace(' ', '_')
print(df.columns)
```

