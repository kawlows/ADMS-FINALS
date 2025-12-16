# ADMS-FINALS: ETL Activity

## Overview
A complete ETL (Extract, Transform, Load) pipeline that consolidates retail data from Japan and Myanmar stores. The system extracts data from CSV files, standardizes currency values (JPY to USD), and combines data from both stores into a unified data warehouse with comprehensive analytics.

## Project Structure

```
ADMS-FINALS/
├── extract.py          
├── transform.py       
├── load.py            
├── analytics.py      
├── etl_database.db   
└── data/             
    ├── source/
    │   ├── japan_store/     
    │   └── myanmar_store/   
    ├── staging/    
    ├── transformation/  
    └── presentation/    
```

## ETL Process

### 1. Extract (extract.py)
Loads CSV files from source directories into SQLite staging tables:
- Processes Japan store files → `japan_*` tables
- Processes Myanmar store files → `myanmar_*` tables
- Handles: customers, branches, items, payments, sales_data

### 2. Transform (transform.py)
Cleans and standardizes data:
- **Data Cleaning**: Removes null values and handles inconsistencies
- **Currency Conversion**: Converts JPY prices to USD using forex rates
  - Fallback rate: 1 USD = 150 JPY
- **Standardization**: Adds store identifiers and currency fields
- Creates transformation area tables

### 3. Load (load.py)
Combines transformed data into unified presentation layer:
- Consolidates Japan and Myanmar sales data
- Creates **BIG_TABLE** - consolidated master table
- Merges sales with item information
- Generates analytics summaries

### 4. Analytics (analytics.py)
Provides 5 Key Insights:

**1. Total Revenue by Store (USD)**
- Compares revenue performance between Japan and Myanmar stores
- Identifies which store generates more revenue

**2. Transaction Volume by Store**
- Shows number of transactions per store
- Indicates store activity levels

**3. Average Transaction Value (USD)**
- Calculates average value per transaction by store
- Reveals pricing patterns and customer spending

**4. Top 3 Best-Selling Items**
- Ranks items by quantity sold
- Identifies popular products across both stores

**5. Market Share Analysis**
- Revenue distribution between stores (percentage)
- Shows competitive positioning

## Database Schema

### Staging Layer
- `japan_*` and `myanmar_*` tables (raw data)

### Transformation Layer
- `*_transformed` tables with cleaned data
- Currency standardized to USD

### Presentation Layer
- **BIG_TABLE**: Master consolidated table
  - Columns: store, transaction_id, item_key, item_id, quantity, item_name, item_price_usd, currency
- **analytics_sales_by_store**: Summary analytics by store

## Getting Started

### Prerequisites
```bash
pip install pandas sqlite3
```

### Running the ETL Pipeline

1. **Extract Data**
   ```bash
   python extract.py
   ```
   Creates SQLite database with staging tables

2. **Transform Data**
   ```bash
   python transform.py
   ```
   Cleans data and converts currencies

3. **Load Data**
   ```bash
   python load.py
   ```
   Creates consolidated BIG_TABLE

4. **Generate Analytics**
   ```bash
   python analytics.py
   ```
   Displays 5 key insights and summary statistics

## Key Features

**Multi-Store Integration**: Combines data from Japan and Myanmar  
**Currency Standardization**: Automatic JPY to USD conversion  
**Data Cleaning**: Removes nulls and handles inconsistencies  
**Consolidated Data Warehouse**: Single unified table for analysis  
**Analytics Generation**: 5 actionable business insights  
**SQLite Database**: Lightweight, file-based storage  
**Modular Design**: Each stage (E-T-L) is independent  

## Technologies
- **Python 3.x**
- **Pandas**: Data manipulation and analysis
- **SQLite**: Database management
- **forex-python**: Currency conversion (with fallback)

## Data Flow

```
Source CSV Files
      ↓
   EXTRACT
      ↓
Staging Layer (SQLite)
      ↓
  TRANSFORM
      ↓
Transformation Layer (Cleaned, Standardized)
      ↓
     LOAD
      ↓
Presentation Layer (BIG_TABLE)
      ↓
  ANALYTICS
      ↓
5 Key Business Insights
```

## File Output

- **etl_database.db**: SQLite database containing all tables
- **Console Output**: Analytics insights printed to terminal

## Notes

- Ensure source CSV files are in `data/source/japan_store/` and `data/source/myanmar_store/` directories
- Database is recreated each run (tables replaced with fresh data)
- Currency conversion uses forex API with 150 JPY/USD fallback
- All prices standardized to USD in final output

## Author
Karl Michael Gaca
