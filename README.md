# Analyzing-Historical-Stock-Revenue-Data-and-Building-a-Dashboard




## Project Overview

In this project, you will assume the role of a **Data Scientist / Data Analyst** working for a startup investment firm. Your job is to **extract financial data** like historical share prices and quarterly revenue reports from various sources using **Python libraries** and **web scraping** on popular stocks. After collecting this data, you will **visualize it in a dashboard** to identify patterns or trends.

### Stocks Analyzed:
- **Tesla (TSLA)**
- **Amazon (AMZN)**
- **AMD (AMD)**
- **GameStop (GME)**

---

## Dashboard Analytics Displayed

A dashboard provides a view of key performance indicators. This project focuses on:
- **Extracting stock data** using `yfinance`
- **Web scraping revenue data** using `BeautifulSoup`
- **Visualizing stock trends** using `Plotly`
- **Building an interactive dashboard** 

We will use **Skills Network Labs** and **Watson Studio** for development.

---

## Project Tasks

### **1. Extract Stock Data using yfinance**
Using `yfinance`, retrieve historical stock data:

```python
import yfinance as yf
import pandas as pd

# Tesla stock data
tsla = yf.Ticker('TSLA')
tesla_data = pd.DataFrame(tsla.history(period='max'))
tesla_data.reset_index(inplace=True)

# Display first 5 rows
tesla_data.head()
```

---

### **2. Web Scrape Tesla Revenue Data**
Using `requests` and `BeautifulSoup`, extract Tesla's quarterly revenue:

```python
import requests
from bs4 import BeautifulSoup

# Download webpage
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
html_data = requests.get(url)

# Parse HTML
soup = BeautifulSoup(html_data.content, 'html.parser')

# Extract table data
tesla_revenue = pd.DataFrame(columns=["Date", "Revenue"])
tbody = soup.find_all("tbody")[1]
for row in tbody.find_all("tr"):
    col = row.findAll("td")
    date = col[0].text
    revenue = col[1].text
    tesla_revenue = pd.concat([tesla_revenue, pd.DataFrame({"Date": [date], "Revenue": [revenue]})], ignore_index=True)

# Clean revenue column
tesla_revenue["Revenue"] = tesla_revenue['Revenue'].str.replace(',|\$', "", regex=True)
tesla_revenue.dropna(inplace=True)
tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]

# Display last 5 rows
tesla_revenue.tail()
```

---

### **3. Extract GameStop Stock Data**
Similar to Tesla, use `yfinance` for GameStop data:

```python
# GameStop stock data
gme = yf.Ticker("GME")
gme_data = pd.DataFrame(gme.history(period='max'))
gme_data.reset_index(inplace=True)

# Display first 5 rows
gme_data.head()
```

---

### **4. Web Scrape GameStop Revenue Data**
Extract GameStop's revenue data:

```python
# Download webpage
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
html_data_2 = requests.get(url).text

# Parse HTML
soup = BeautifulSoup(html_data_2, 'html.parser')

# Extract table data
gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])
tbody = soup.find_all("tbody")[1]
for row in tbody.find_all("tr"):
    col = row.findAll("td")
    date = col[0].text
    revenue = col[1].text
    gme_revenue = pd.concat([gme_revenue, pd.DataFrame({"Date": [date], "Revenue": [revenue]})], ignore_index=True)

# Clean revenue column
gme_revenue['Revenue'] = gme_revenue['Revenue'].str.replace(',|\$', '', regex=True).astype(float)
gme_revenue.dropna(subset=['Revenue'], inplace=True)

# Display last 5 rows
gme_revenue.tail()
```

---

### **5. Plot Tesla Stock Graph**
Use `make_graph` function to visualize Tesla's stock trends:

```python
make_graph(tesla_data, tesla_revenue, 'Tesla')
```

---

### **6. Plot GameStop Stock Graph**
Use `make_graph` function to visualize GameStop's stock trends:

```python
make_graph(gme_data, gme_revenue, 'GameStop')
```

---

## Technologies Used
- **Python** (yfinance, pandas, requests, BeautifulSoup, Plotly)
- **Jupyter Notebook**
- **Watson Studio**
- **Skills Network Labs**

---

## Conclusion
This project demonstrates how to **extract, clean, analyze, and visualize stock market data** using Python. By web scraping revenue reports and integrating historical stock data, we can build **interactive dashboards** to track stock trends.

---
