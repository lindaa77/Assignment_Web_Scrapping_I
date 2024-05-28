# Assignment-Webscraping-1

## **BeautifulSoup**

This README provides a detailed guide on how to scrape data from a webpage using BeautifulSoup, process the HTML content, and extract table data into a CSV file. 

### Prerequisites

Before starting, ensure you have the following Python libraries installed:

- `requests`: For sending and receiving HTTP requests.
- `beautifulsoup4`: For parsing and navigating HTML and XML web pages.
- `pandas`: For data manipulation and analysis.

You can install these libraries using pip:

```bash
pip install requests beautifulsoup4 pandas
```

### Sections

1. **Import Required Modules**
2. **Get Response from URL**
3. **Parse & Print HTML with BeautifulSoup**
4. **Get Table Data from BeautifulSoup**
5. **Extract Table Headers and Rows Data**
6. **Clean the Data (Optional)**
7. **Save Data to CSV File**

### 1. Import Required Modules

First, import the necessary libraries:

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

- `requests`: Used for sending HTTP requests to retrieve web content.
- `BeautifulSoup`: Used for parsing and navigating HTML content.
- `pandas`: Used for data manipulation and analysis.

### 2. Get Response from URL

Send a request to the webpage and print the response:

```python
# URL for the webpage to be scraped
url = 'https://www.petsecure.com.au/cover-choices/'

# Send a request to the webpage
response = requests.get(url)

# Print response
print(response)
```

- `url`: The URL of the webpage to scrape.
- `requests.get(url)`: Sends a GET request to the specified URL.
- `response`: Stores the server's response to the HTTP request.
- `print(response)`: Outputs the HTTP response status (e.g., 200 for a successful request).

### 3. Parse & Print HTML with BeautifulSoup

Parse the HTML content using BeautifulSoup and print it in a readable format:

```python
# Parse the HTML content using BeautifulSoup
soup = BeautifulSoup(response.content, 'html.parser')

# Print the parsed HTML content in a readable format
print(soup.prettify())
```

- `BeautifulSoup(response.content, 'html.parser')`: Parses the HTML content of the webpage.
- `soup.prettify()`: Formats the parsed HTML content to be more readable.

### 4. Get Table Data from BeautifulSoup

Locate the specific table within the HTML content. In this example, we are looking for a `div` with class `table-2`:

```python
# Find the div with class 'table-2'
assurance = soup.find('div', {'class': 'table-2'})

# Find the table within the div
table = assurance.find('table')

# Print the table HTML content in a readable format
print(table.prettify())
```

- `soup.find('div', {'class': 'table-2'})`: Finds the `div` element with class `table-2`.
- `assurance.find('table')`: Finds the `table` element within the located `div`.
- `print(table.prettify())`: Formats and prints the table's HTML content.

### 5. Extract Table Headers and Rows Data

Extract the headers and rows from the table:

```python
# Extract the table headers
headers = [th.text.strip() for th in table.find_all('th')]
print(headers)

# Extract the table data
data = []
for tr in table.find_all('tr'):
    row_data = [td.text.strip() for td in tr.find_all('td')]
    if row_data:  # Only add non-empty rows
        data.append(row_data)

print(data)
```

- `table.find_all('th')`: Finds all `th` elements (table headers) in the table.
- `th.text.strip()`: Extracts and strips whitespace from the text within each `th` element.
- `headers`: A list of the table's header texts.
- `table.find_all('tr')`: Finds all `tr` elements (table rows) in the table.
- `tr.find_all('td')`: Finds all `td` elements (table data cells) within each row.
- `td.text.strip()`: Extracts and strips whitespace from the text within each `td` element.
- `data`: A list of lists, where each inner list represents a row of table data.

### 6. Clean the Data (Optional)

Convert the extracted data into a DataFrame and clean it as necessary:

```python
# Convert list to DataFrame
df = pd.DataFrame(data, columns=headers)
print(df)

# Check dataframe info
print(df.info())

# Drop any columns or rows with unwanted data
df.drop(columns=[''], inplace=True, errors='ignore')
df.dropna(subset=['Benefits'], inplace=True)
df.reset_index(drop=True, inplace=True)
print(df)
```

- `pd.DataFrame(data, columns=headers)`: Creates a DataFrame from the extracted data with headers as column names.
- `df.info()`: Displays summary information about the DataFrame.
- `df.drop(columns=[''], inplace=True, errors='ignore')`: Drops any empty columns.
- `df.dropna(subset=['Benefits'], inplace=True)`: Drops rows where the 'Benefits' column is empty.
- `df.reset_index(drop=True, inplace=True)`: Resets the DataFrame index after dropping rows.

### 7. Save Data to CSV File

Finally, save the cleaned data to a CSV file:

```python
df.to_csv('result.csv', index=False, sep=';')
```

- `df.to_csv('result.csv', index=False, sep=';')`: Saves the DataFrame to a CSV file named 'result.csv' with semicolons as separators and without the DataFrame index.

## References

* [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
* [Requests Documentation](https://docs.python-requests.org/en/master/)
* [Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/)
* [Web Scraping Best Practices](https://www.scrapingbee.com/blog/web-scraping-best-practices/)
