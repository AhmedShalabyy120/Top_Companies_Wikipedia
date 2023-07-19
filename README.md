# Web Scraping and DataFrame Creation

This Python code demonstrates how to scrape data from a web page using BeautifulSoup library and create a DataFrame using pandas library.

## Description

The code retrieves the list of largest companies by revenue from the Wikipedia page (https://en.wikipedia.org/wiki/List_of_largest_companies_by_revenue). It scrapes the required data using the requests and BeautifulSoup libraries, and then creates a pandas DataFrame with the scraped data.

## Dependencies

The following dependencies are required to run the code:

- pandas
- requests
- BeautifulSoup

You can install these dependencies using pip:

## Usage

1. Ensure that the dependencies are installed.
2. Run the code in a Python environment.

```python
import pandas as pd
import requests
from bs4 import BeautifulSoup as bs

# Fetch the HTML content from the web page
response = requests.get('https://en.wikipedia.org/wiki/List_of_largest_companies_by_revenue').text

# Create a BeautifulSoup object to parse the HTML
soup = bs(response, 'lxml')

# Extract the column names
columns = [column.text.strip() for column in table('th')[:7]]

# Create an empty DataFrame with the extracted column names
df = pd.DataFrame(columns=columns)

# Rename the column 'Headquarters[note 1]' to 'Headquarters'
df = df.rename(columns={'Headquarters[note 1]': 'Headquarters'})

# Remove the 'Rank' column from the DataFrame
df.drop('Rank', axis=1, inplace=True)

# Extract all the data rows from the table
all_data = table.find_all('tr')

# Iterate over the rows and extract the row data
for i in all_data[2:]:
    row_data = i.find_all('td')
    allrows = [row.text.strip() for row in row_data[:-2]]
    length = len(df)
    df.loc[length] = allrows

# Reset the index and assign 'Rank' as the index name
df = df.rename_axis('Rank').reset_index()

