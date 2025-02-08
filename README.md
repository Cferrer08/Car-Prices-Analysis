# Car Analysis Dashboard

###Dashboard Demo:
https://drive.google.com/file/d/1hX1jGZdqw-LB81RQgrEqzYMpl5HHIkOB/view?usp=sharing

## Overview
This project analyzes car listings from **Supercarros.com** by scraping, cleaning, and visualizing the data in **Power BI**. The dataset includes information such as price, brand, model, odometer readings, fuel type, and range.

## Data Collection
- **Source:** Supercarros.com
- **Method:** Web scraping using Python & Selenium
- **Challenges:**
  - Created a **base URL** for the main page and **parameterized URLs** to extract data by brand ID.
  - Handled dynamic content loading.

- Code snippet:
  ```python
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))

    base_url = "https://www.supercarros.com/buscar/"
    base_params = "?do=1&ObjectType=1&PriceFrom=0&PriceTo=50000000&YearFrom=1970&YearTo=2025&Brand="

    driver.get("https://www.supercarros.com/carros/cualquier-tipo/cualquier-provincia/ver-todos/")
    time.sleep(3)

    try:
    WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.CSS_SELECTOR, "ul.bigsearch-filters-box-list li"))
    )
    print("Brand list loaded.")
    except Exception as e:
    print(f"Error loading brand list: {e}")
    driver.quit()
    exit()

## Data Cleaning
After scraping, the dataset required multiple cleaning steps:

### Price Cleaning
- Extracted numeric values from the "Price" column by removing text (`US$`, `,` symbols).
- Converted all values to USD using the exchange rate **1 USD = 61.3 DOP**.
- Code snippet:
  ```python
  import pandas as pd
  
  # Remove non-numeric characters and convert to float
  df['placeholder'] = df['Price'].replace({',': ''}, regex=True).str.extract(r'(\d+(\.\d+)?)')[0].astype(float)
  
  # Convert to USD based on prefix
  df['Price_in_USD'] = df.apply(
      lambda row: row['placeholder'] if str(row['Price']).startswith("US") else row['placeholder'] // 61.3,
      axis=1
  )

### Brand and Vehicle Name Cleaning
- The "Vehicle Name" column included the brand, leading to redundancy.
- Implemented a function that checks if the brand appears in the vehicle name and removes it.

### Mileage Extraction
- The "Details" column contained multiple values (Year, Power, Usage, Mileage) separated by " - ".
- Extracted each value into separate columns (Year, Power, Usage, Odometer).

## Power BI Dashboard
The cleaned dataset was loaded into Power BI to create interactive visualizations:

### Key Insights & Visuals
- **Total Cars & Models:** Displayed as KPI cards.
- **Price Trends Over Time:** Line chart showing how car prices change by year.
- **Top 5 Electric Cars by Range:** Bar chart highlighting the electric cars with the highest range.
- **Fuel Type Distribution:** Pie chart showing the proportion of Gasoline, Diesel, Hybrid, Electric, and GLP cars.
- **Average Price by Model:** Horizontal bar chart comparing high-end car prices.

### Dashboard Screenshot

![Image](https://github.com/user-attachments/assets/c334ad06-eae3-44b8-85f0-9eeccdd89d45)


## How to Reproduce
1. Clone this repository:
   ```bash
   git clone https://github.com/Cferrer08/Car-Prices-Analysis.git
   cd Car-Prices-Analysis

2. Install dependencies:
   ```bash
    pip install selenium pandas openpyxl

3. Run Scrapper:
   ```bash
    Supercarros - General Info x Brand.ipynb

4. Run Data Cleaner:
   ```bash
    Cars_DataClean.ipynb

### Future Improvements
- **Enhance the Scraper:** Expand the scraper to fetch additional car attributes such as transmission type, color, and interior features.
- **Automate Data Updates:** Implement a scheduling system (e.g., using cron jobs or a task scheduler) to automatically update the dataset at regular intervals for real-time analysis.
- **Machine Learning Models:** Develop machine learning models to predict car prices based on attributes like brand, model, mileage, and fuel type.



## Author

- [@Carlos Ferrer](https://github.com/Cferrer08)



📧 If you need further assistance or have any questions, feel free to contact me at: [carlosferrerg08@gmail.com](mailto:carlosferrerg08@gmail.com)
