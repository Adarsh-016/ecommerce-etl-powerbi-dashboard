# 🛒 Ecommerce Sales Analysis — Data Cleaning Pipeline & Power BI Dashboard

An end-to-end data project that takes a **messy, real-world ecommerce export**, cleans and transforms it with Python (pandas), loads it into a **MySQL/MariaDB** database, and powers an interactive **Power BI dashboard** for sales, profit, and order analysis.

![Ecommerce Sales Analysis Dashboard](assets/dashboard-preview.png)

---

## 📊 Dashboard Highlights

| Metric | Value |
|---|---|
| Total Sales | 3M |
| Total Profit | 647.10K |
| Total Orders | 358 |
| Average Order Value | 9.04K |

The dashboard breaks sales and profit down by **month, category, product, payment mode, city, and order status**, with slicers for Month, City, and Payment Mode for interactive filtering.

---

## 🧱 Project Architecture

```
Raw Excel (unclean)  →  Python/pandas cleaning  →  MySQL/MariaDB  →  Power BI dashboard
                                    │
                                    └──→  Clean CSV (alternate source for Power BI)
```

## 📁 Repository Structure

```
.
├── assets/
│   └── dashboard-preview.png       # Power BI dashboard screenshot
├── ecommerce_pipeline.ipynb        # Main notebook: connect, clean, load
├── Ecommerce_Unclean_Project.xlsx  # Raw, unclean source data
├── Clean_Ecommerce.csv             # Cleaned, analysis-ready output
├── requirements.txt                # Python dependencies
├── .env.example                    # Template for local DB credentials
└── .gitignore
```

## 🗂️ Dataset

The cleaned dataset (`Clean_Ecommerce.csv`) contains **358 orders** across **21 columns**:

`Order_ID, Order_Date, Customer_Name, Email, Phone, City, State, Product, Category, Qty, Unit_Price, Discount, Payment_Mode, Order_Status, Delivery_Date, Sales, Net_Amount, Profit, Month, Year, Weekday`

**Categories:** Electronics, Fashion, Furniture, Home
**Payment modes:** Card, NetBanking, COD, Wallet, UPI
**Order statuses:** Delivered, Pending, Cancelled, Returned

## 🧹 Data Cleaning Steps

The pipeline (in `ecommerce_pipeline.ipynb`) applies the following transformations to the raw file:

1. **Standardize column names** — strip stray whitespace.
2. **Normalize text fields** — trim whitespace, convert blank/`"None"`/`"NaT"` strings to proper nulls.
3. **Remove duplicates** — both full-row duplicates and duplicate `Order_ID`s (keeping the first).
4. **Drop incomplete records** — rows missing an `Order_ID` or `Order_Date`.
5. **Parse dates correctly** — day-first format (`dd-mm-yyyy`), coercing invalid dates to null and dropping them.
6. **Clean numeric fields** — coerce `Qty`, `Unit_Price`, and `Discount` to numeric, dropping rows with invalid quantity/price.
7. **Derive financial metrics** — compute `Sales`, `Net_Amount`, and `Profit` where not already present.
8. **Derive calendar fields** — `Month`, `Year`, and `Weekday` extracted from `Order_Date`.
9. **Load to SQL** — cleaned data is pushed to a `ecommerce_data` table in MySQL/MariaDB for Power BI to consume, and also exported as a CSV.

> **Note:** The profit calculation uses a **20% placeholder margin** where no actual profit column exists in the source data — swap this for your real cost/profit figures if available.

## 🛠️ Tech Stack

- **Python** — pandas, NumPy for data cleaning
- **SQLAlchemy + PyMySQL** — database connectivity
- **MySQL / MariaDB** — data warehouse
- **Power BI** — dashboard & visualization
- **python-dotenv** — secure local credential management

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure your database credentials
Copy the example env file and fill in your own values:
```bash
cp .env.example .env
```
```env
DB_USER=root
DB_PASSWORD=your_password_here
DB_HOST=localhost
DB_PORT=3306
```

### 4. Run the pipeline
Open `ecommerce_pipeline.ipynb` in Jupyter and run all cells in order. This will:
- Create the `ecommerce` database (if it doesn't exist)
- Load and clean `Ecommerce_Unclean_Project.xlsx`
- Push the cleaned data into the `ecommerce_data` table
- Export `Clean_Ecommerce.csv`

### 5. Connect Power BI
Point Power BI at either:
- The `ecommerce_data` table in your MySQL/MariaDB instance, **or**
- The `Clean_Ecommerce.csv` file directly

## 🔒 Security

Database credentials are **never hardcoded**. They're loaded from a local `.env` file via `python-dotenv`, and `.env` is excluded from version control via `.gitignore`. Only `.env.example` (a template with no real secrets) is committed.

## 📈 Key Insights from the Dashboard

- **Electronics** is the top-performing category by sales (1.48M), followed by Fashion and Furniture.
- **Mouse, Laptop, and Chair** are the top three products by sales volume.
- Sales show a **declining trend** from February through June in this dataset.
- Payment methods are fairly evenly distributed, with **UPI (22.96%)** slightly ahead of Card, COD, and NetBanking.
- Order status is split across Delivered, Pending, Cancelled, and Returned in roughly even proportions — worth investigating cancellation/return drivers further.

## 📄 License

This project is open source and available for personal and educational use.
