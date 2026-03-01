
# 🌐 Web Scraping: Largest Companies by Revenue

A Python web scraping project that extracts data about the world's largest companies by revenue from Wikipedia and stores it in a structured pandas DataFrame.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-43B02A?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

---

## 📑 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Data Extracted](#data-extracted)
- [Installation](#installation)
- [Usage](#usage)
- [Code Breakdown](#code-breakdown)
- [Sample Output](#sample-output)
- [Technologies Used](#technologies-used)
- [Key Learnings](#key-learnings)
- [Future Enhancements](#future-enhancements)
- [Legal & Ethical Considerations](#legal--ethical-considerations)
- [Troubleshooting](#troubleshooting)
- [Author](#author)

---

## 🎯 Project Overview

This project demonstrates web scraping techniques by extracting real-time data about the world's top 50 largest companies by revenue from Wikipedia. The scraped data is cleaned, structured, and stored in a pandas DataFrame for further analysis.

**Data Source**: [Wikipedia - List of Largest Companies by Revenue](https://en.wikipedia.org/wiki/List_of_largest_companies_by_revenue)

**Purpose**: 
- 📊 Learn web scraping fundamentals
- 🔍 Extract structured data from HTML tables
- 🧹 Practice data cleaning and transformation
- 📈 Analyze global business trends

---

## ✨ Features

- ✅ **Automated Data Extraction**: Scrapes Wikipedia table automatically
- ✅ **Structured Data**: Organizes data into clean pandas DataFrame
- ✅ **Top 50 Companies**: Extracts information about the largest companies globally
- ✅ **Multiple Attributes**: Captures rank, name, industry, revenue, profit, employees, headquarters, and more
- ✅ **User-Agent Handling**: Properly identifies requests to avoid blocking
- ✅ **Data Cleaning**: Removes unnecessary characters and formats data
- ✅ **Export Ready**: Easy to export to CSV, Excel, or database
- ✅ **Jupyter Notebook**: Interactive and easy to modify

---

## 📊 Data Extracted

### Columns in Dataset

| Column | Description |
|--------|-------------|
| **Rank** | Company's position in the list (1-50) |
| **Name** | Official company name |
| **Industry** | Primary business sector |
| **Revenue** | Annual revenue in USD |
| **Profit** | Annual profit in USD |
| **Employees** | Total number of employees |
| **Headquarters** | Country where company is headquartered |
| **State-owned** | Whether the company is state-owned |
| **Ref** | Reference citations |

### Sample Companies Included

- **Walmart** - Retail
- **Amazon** - Retail/Information Technology
- **Saudi Aramco** - Oil and Gas
- **Apple** - Information Technology
- **State Grid Corporation of China** - Electricity
- **Alphabet (Google)** - Information Technology
- And 44+ more global giants!

---

## 🚀 Installation

### Prerequisites

- Python 3.x
- pip (Python package manager)
- Jupyter Notebook (optional)

### Required Libraries

```bash
pip install beautifulsoup4
pip install requests
pip install pandas
pip install lxml
```

Or install all at once:

```bash
pip install beautifulsoup4 requests pandas lxml
```

### Clone the Repository

```bash
git clone https://github.com/CODERGURU26/Web-Scraping-Largest-Companies.git
cd Web-Scraping-Largest-Companies
```

---

## 💻 Usage

### Method 1: Jupyter Notebook (Recommended)

1. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook
   ```

2. **Open the Notebook**
   - Navigate to `Web_Scraping_of_List_of_largest_companies_by_revenue.ipynb`

3. **Run All Cells**
   - Execute cells sequentially
   - View the extracted data in the final DataFrame

4. **Export Data (Optional)**
   ```python
   # Save to CSV
   DF.to_csv('largest_companies.csv', index=False)
   
   # Save to Excel
   DF.to_excel('largest_companies.xlsx', index=False)
   ```

### Method 2: Python Script

Convert the notebook to a Python script and run:

```bash
python web_scraper.py
```

---

## 🧩 Code Breakdown

### 1. Import Required Libraries

```python
from bs4 import BeautifulSoup
import requests
import pandas as pd
```

- **BeautifulSoup**: Parses HTML and extracts data
- **requests**: Fetches web pages
- **pandas**: Structures data into DataFrame

### 2. Set Up Request Headers

```python
url = 'https://en.wikipedia.org/wiki/List_of_largest_companies_by_revenue'

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
}

page = requests.get(url, headers=headers)
soup = BeautifulSoup(page.text, 'html.parser')
```

**Why User-Agent?**
- Identifies the request as coming from a browser
- Prevents potential blocking by the server
- Follows web scraping best practices

### 3. Locate the Target Table

```python
revenue_table = soup.find_all('table')[0]
```

- Finds all HTML tables on the page
- Selects the first table (index 0) containing company data

### 4. Extract Table Headers

```python
title_rows = revenue_table.find_all("tr")[0]
revenue_titles = [title.text.strip() for title in title_rows.find_all('th')]
```

**Output:**
```python
['Ranks', 'Name', 'Industry', 'Revenue', 'Profit', 'Employees', 
 'Headquarters[note 1]', 'State-owned', 'Ref.']
```

### 5. Extract Table Data

```python
revenue_table_body = revenue_table.find_all('tbody')[0]
data_rows = revenue_table_body.find_all('tr')

table_data = []
for row in data_rows[1:]:  
    columns = row.find_all(['th', 'td'])
    row_data = [col.get_text(strip=True) for col in columns]
    table_data.append(row_data)
```

**Process:**
- Skips header row (`[1:]`)
- Extracts both `<th>` and `<td>` elements
- Strips whitespace from text
- Appends each row to list

### 6. Create DataFrame

```python
DF = pd.DataFrame(table_data, columns=[
    "Rank", "Name", "Industry", "Revenue",
    "Profit", "Employees", "Headquarters",
    "State-owned", "Ref"
])
```

### 7. Clean Data

```python
DF = DF.drop(index=0).reset_index(drop=True)
```

- Removes duplicate header row if present
- Resets index to start from 0

---

## 📈 Sample Output

```
Rank  Name                              Industry         Revenue    Profit     Employees   Headquarters
1     Walmart                          Retail           $680,985   $19,436    2,100,000   United States
2     Amazon                           Retail/IT        $637,959   $59,248    1,556,000   United States
3     State Grid Corporation of China  Electricity      $545,948   $9,204     1,361,423   China
4     Saudi Aramco                     Oil and gas      $480,446   $106,246   73,311      Saudi Arabia
5     Alphabet (Google)                IT               $402,872   $132,170   190,820     United States
...
```

### Top 10 Companies by Revenue (2024)

1. 🛒 **Walmart** - $680.9B
2. 📦 **Amazon** - $638.0B
3. ⚡ **State Grid Corporation of China** - $545.9B
4. 🛢️ **Saudi Aramco** - $480.4B
5. 🛢️ **China National Petroleum** - $476.0B
6. 🛢️ **China Petrochemical** - $429.7B
7. 🔍 **Alphabet** - $402.9B
8. 🏥 **UnitedHealth Group** - $400.3B
9. 🍎 **Apple** - $391.0B
10. 💼 **Berkshire Hathaway** - $371.4B

---

## 🛠️ Technologies Used

| Technology | Purpose |
|------------|---------|
| **Python 3.x** | Programming language |
| **BeautifulSoup4** | HTML parsing and data extraction |
| **Requests** | HTTP library for fetching web pages |
| **Pandas** | Data manipulation and analysis |
| **Jupyter Notebook** | Interactive development environment |
| **lxml** | XML and HTML parser (optional) |

---

## 🎓 Key Learnings

This project demonstrates proficiency in:

- ✅ **Web Scraping Fundamentals**: Extracting data from HTML
- ✅ **HTTP Requests**: Using headers and handling responses
- ✅ **HTML Parsing**: Navigating DOM structure with BeautifulSoup
- ✅ **Data Cleaning**: Removing duplicates and formatting
- ✅ **Pandas Operations**: Creating and manipulating DataFrames
- ✅ **List Comprehensions**: Efficient Python coding
- ✅ **Error Handling**: Managing potential scraping issues

---

## 🔮 Future Enhancements

### Data Analysis Features
- [ ] Add revenue growth analysis (year-over-year)
- [ ] Calculate profit margins for each company
- [ ] Visualize data with matplotlib/seaborn
- [ ] Create industry-wise revenue comparison
- [ ] Geographic revenue distribution analysis

### Technical Improvements
- [ ] Add error handling and retry logic
- [ ] Implement data validation
- [ ] Schedule automatic scraping (daily/weekly)
- [ ] Add logging functionality
- [ ] Create data versioning system
- [ ] Export to multiple formats (JSON, SQLite)

### Advanced Features
- [ ] Compare data across multiple years
- [ ] Scrape additional company details
- [ ] Create interactive dashboard (Streamlit/Dash)
- [ ] Add email notifications for data updates
- [ ] Implement caching mechanism
- [ ] Build REST API to serve data

---

## ⚖️ Legal & Ethical Considerations

### Wikipedia's Terms of Use
- ✅ Wikipedia allows web scraping for personal/educational use
- ✅ Data is available under Creative Commons license
- ✅ Always check `robots.txt` before scraping
- ✅ Respect rate limits and don't overload servers

### Best Practices
1. **Use Appropriate User-Agent**: Identify your scraper
2. **Respect robots.txt**: Check allowed/disallowed paths
3. **Rate Limiting**: Add delays between requests if scraping multiple pages
4. **Attribution**: Credit Wikipedia as data source
5. **Personal Use**: Don't republish scraped data commercially without permission

### robots.txt Check
```python
# Always check robots.txt
# https://en.wikipedia.org/robots.txt
```

---

## 🐛 Troubleshooting

### Common Issues and Solutions

#### Issue 1: Connection Error
**Error:** `ConnectionError` or `Timeout`

**Solution:**
```python
# Add timeout parameter
page = requests.get(url, headers=headers, timeout=10)

# Add retry logic
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry

session = requests.Session()
retry = Retry(connect=3, backoff_factor=0.5)
adapter = HTTPAdapter(max_retries=retry)
session.mount('http://', adapter)
session.mount('https://', adapter)
```

#### Issue 2: Empty DataFrame
**Error:** DataFrame has no data

**Solution:**
- Verify table index: `soup.find_all('table')[0]` might need adjustment
- Check if Wikipedia page structure changed
- Print intermediate variables to debug

#### Issue 3: 403 Forbidden Error
**Error:** `403 Client Error: Forbidden`

**Solution:**
- Ensure User-Agent header is set
- Add more headers (Accept, Accept-Language)
- Use different User-Agent string

#### Issue 4: Import Errors
**Error:** `ModuleNotFoundError`

**Solution:**
```bash
# Reinstall packages
pip install --upgrade beautifulsoup4 requests pandas lxml
```

---

## 📊 Data Analysis Examples

### Example 1: Top 10 Companies by Revenue
```python
top_10 = DF.head(10)
print(top_10[['Rank', 'Name', 'Revenue']])
```

### Example 2: Companies by Industry
```python
industry_count = DF['Industry'].value_counts()
print(industry_count)
```

### Example 3: Average Revenue by Industry
```python
# Clean revenue column (remove $ and commas)
DF['Revenue_Clean'] = DF['Revenue'].str.replace('$', '').str.replace(',', '').astype(float)
avg_revenue = DF.groupby('Industry')['Revenue_Clean'].mean().sort_values(ascending=False)
print(avg_revenue)
```

### Example 4: Filter by Country
```python
us_companies = DF[DF['Headquarters'].str.contains('United States', na=False)]
print(f"Number of US companies in top 50: {len(us_companies)}")
```

---

## 📂 Project Structure

```
Web-Scraping-Largest-Companies/
│
├── Web_Scraping_of_List_of_largest_companies_by_revenue.ipynb
│   └── Main Jupyter Notebook with scraping code
│
├── largest_companies.csv (generated after export)
│   └── Exported data in CSV format
│
├── requirements.txt
│   └── List of required Python packages
│
└── README.md
    └── Project documentation (this file)
```

---

## 📦 Export Options

### CSV Export
```python
DF.to_csv('largest_companies.csv', index=False, encoding='utf-8')
```

### Excel Export
```python
DF.to_excel('largest_companies.xlsx', index=False, sheet_name='Top 50 Companies')
```

### JSON Export
```python
DF.to_json('largest_companies.json', orient='records', indent=2)
```

### SQLite Database
```python
import sqlite3

conn = sqlite3.connect('companies.db')
DF.to_sql('largest_companies', conn, if_exists='replace', index=False)
conn.close()
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. **Commit your changes**
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. **Push to the branch**
   ```bash
   git push origin feature/AmazingFeature
   ```
5. **Open a Pull Request**

### Ideas for Contributions
- Add data visualization charts
- Implement automatic scheduling
- Create comparison with previous years
- Add more company metrics
- Improve error handling
- Create unit tests

---

## 📄 License

**Data License**: Wikipedia data is available under [Creative Commons Attribution-ShareAlike License](https://creativecommons.org/licenses/by-sa/3.0/).

---

## 📧 Contact & Connect

### Author

**Gururaj Krishna Sharma**

- 📧 Email: [guruuu2468@gmail.com](mailto:guruuu2468@gmail.com)
- 💼 LinkedIn: [Gururaj Krishna Sharma](https://www.linkedin.com/in/gururaj-krishna-sharma)
- 💻 GitHub: [@CODERGURU26](https://github.com/CODERGURU26)

---

## 🌟 Acknowledgments

- **Wikipedia** for providing open access to data
- **BeautifulSoup** developers for the excellent parsing library
- **Pandas** community for data manipulation tools
- **Python** community for extensive documentation

---

## 📚 Additional Resources

### Learn More About Web Scraping
- [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Requests Library Guide](https://requests.readthedocs.io/)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Web Scraping Best Practices](https://www.scraperapi.com/blog/web-scraping-best-practices/)

### Related Projects
- Stock price scrapers
- Real estate data extraction
- Job listing aggregators
- News article collectors

---

## 🎯 Use Cases

### For Students
- Learn web scraping fundamentals
- Practice data cleaning techniques
- Build portfolio project

### For Data Analysts
- Quick access to company revenue data
- Competitive analysis research
- Industry trend analysis

### For Researchers
- Economic research data
- Corporate structure analysis
- Global business trends study

### For Developers
- Learn Python libraries
- Practice API alternatives
- Build automation skills

---

**⭐ If you find this project helpful, please give it a star!**

---

*Last Updated: February 2026*

**Disclaimer**: This project is for educational purposes only. Always respect website terms of service and rate limits when scraping data.
