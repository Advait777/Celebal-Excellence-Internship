# Week 1 – Basic Data Exploration and Cleaning using Pandas

**Celebal Technologies | Data Engineering Internship**

## Objective
Learn Python basics and perform foundational data exploration and cleaning using Pandas — loading data, exploring its structure, handling missing values, filtering/selecting data, removing duplicates, engineering a derived column, and exporting the cleaned result.

## Dataset
`Combined_dataset.csv` — a product-listing dataset containing 1,000 fashion/accessory products scraped across different categories (tops, dresses, jeans, sports shoes, backpacks, etc.), with fields such as price, discount, rating, seller information, and product specifications.

## Approach
1. Loaded the raw CSV into a Pandas DataFrame
2. Explored structure: `head()`/`tail()`, shape, columns, dtypes, `info()`
3. Identified and handled missing values across 4 columns and dropped 2 irrelevant columns
4. Demonstrated column selection and row filtering (single condition, multiple conditions, combined filter+select)
5. Cleaned `final_price` to numeric from string
5. Investigated duplicates
7. Engineered `quantity` by filling integer values randomly (documented assumption) and `total_amount = final_price * quantity`
8. Saved the cleaned dataset to `processed_data/Combined_dataset_cleaned.csv`

## Key Decisions & Findings
- **Missing values weren't random.** A blind `dropna()` would have discarded **99% of the dataset** (1000 → 10 rows). Instead, each column was filled based on what its absence actually meant (e.g. missing `discount` → no discount was applied, validated against `final_price == initial_price` in 92% of those rows; missing `videos`/`variations`/`what_customers_said` → the product simply doesn't have that feature, not a data error).
- **`quantity` is synthetic and documented**, not real — random integers 1–5 (fixed seed for reproducibility), since this catalog has no transactional quantity data. `total_amount` uses `final_price` (price actually paid after discount), not `initial_price`.

## Results

- Missing values resolved in: `discount`, `seller_name`, `seller_information`, `what_customers_said` 
- Columns dropped: `videos`, `variations`
- Final missing values: **0**
- Columns added: `quantity`, `total_amount`

## Folder Structure
```
Week1_Data_Exploration_and_Cleaning/
├── W1_Data_Exploration_Cleaning.ipynb   
├── raw_data/
│   └── Combined_dataset.csv          
├── processed_data/
│   └── Combined_dataset_cleaned.csv  
└── README.md
```

## How to Run
1. Open `data_exploration_cleaning.ipynb` in Jupyter (run from within the `Week1_data-exploration-cleaning/` folder, since file paths are relative)
2. Run all cells top to bottom
3. Cleaned output will be written to `processed_data/Combined_dataset_cleaned.csv`

**Requirements:** `pandas`, `numpy`
