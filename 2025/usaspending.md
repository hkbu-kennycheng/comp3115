
# USAspending and its API with Python

It's a very hot topic in the US, and it's a very interesting topic for data scientists. The USAspending API provides access to a wealth of data on federal spending. This data can be used to analyze trends in federal spending, identify opportunities for cost savings, and track the impact of federal spending on the economy. The API can be used by researchers, journalists, and the general public to gain insights into how the federal government spends taxpayer dollars.

## Introduction

[USAspending.gov](https://www.usaspending.gov/) is the publicly accessible, searchable website mandated by the Federal Funding Accountability and Transparency Act of 2006 to give the American public access to information on how their tax dollars are spent. The website is maintained by the Department of the Treasury. The USAspending API provides programmatic access to the data on the website. That's what we are going to explore in this lab.

## Motivation

The USAspending API provides access to a wealth of data on federal spending. This data can be used to analyze trends in federal spending, identify opportunities for cost savings, and track the impact of federal spending on the economy. The API can be used by researchers, journalists, and the general public to gain insights into how the federal government spends taxpayer dollars.

## Data

The USAspending API provides access to a wide range of data on federal spending. This includes data on federal contracts, grants, loans, and other financial assistance. The data is organized into several categories, including awarding agencies, recipients, and award amounts. The API also provides access to data on federal spending by state, county, and congressional district.

## Accessing the API

The USAspending API is a RESTful API that accepts and returns JSON data. The API provides several endpoints for accessing different types of data on federal spending. To access the API, you don't need any special credentials. You can access the API using a web browser, a command-line tool like curl, or a programming language like Python.

To see the full list of available endpoints, you can visit the [USAspending API documentation](https://api.usaspending.gov/docs/endpoints/). Let's explore some of the endpoints in the next section.

## A walk-through of the API endpoints by examples

The following examples demonstrates how to use the USAspending API to retrieve data on federal contracts awarded by the Department of Defense. The example uses Python and the requests library to make a GET or POST request to the API endpoint for retrieving contract data. The example then prints the first 10 contracts in the response.

### Search for awarding agencies data

In federal spending data, awarding agencies are the federal agencies that award contracts, grants, loans, or other financial assistance. There are hundreds of awarding agencies in the federal government. To search for awarding agencies with text, we can use the `POST /api/v2/autocomplete/awarding_agency/` endpoint. Here is an example using Python:

```python
import requests

url = 'https://api.usaspending.gov/api/v2/autocomplete/awarding_agency/'
data = {'search_text': 'US', 'limit': 500}
response = requests.post(url, json=data)
data = response.json()
print(data)
```

- `search_text`: The text to search for awarding agencies. In this example, we are searching for awarding agencies whose names contain the letter 'U'.
- `limit`: The maximum number of results to return. In this example, we are limiting the results to 500 which is the maximum number for limit.

A successful response will return a JSON object with a `results` key containing a list of awarding agencies that match the search text. Each awarding agency object will contain information such as the agency name, abbreviation, and ID in alphabetical order of the agency name.

```json
{
  "results": [
    {
        "id": 1136,
        "toptier_flag": true,
        "toptier_agency": {
            "toptier_code": "302",
            "abbreviation": "ACUS",
            "name": "Administrative Conference of the U.S."
        },
        "subtier_agency": {
            "abbreviation": "ACUS",
            "name": "Administrative Conference of the U.S."
        }
    }
    {
        "id": 1527,
        "toptier_flag": true,
        "toptier_agency": {
            "toptier_code": "166",
            "abbreviation": "USADF",
            "name": "African Development Foundation"
        },
        "subtier_agency": {
            "abbreviation": "USADF",
            "name": "African Development Foundation"
        }
    },
    ...
  ]
}
```

You could take a look at the full documentation of this API endpoint [here](https://github.com/fedspendingtransparency/usaspending-api/blob/master/usaspending_api/api_contracts/contracts/v2/autocomplete/awarding_agency.md)

#### Convert results JSON string to Pandas DataFrame

After getting the result JSON, you can convert it to a Pandas DataFrame to analyze the data more easily. Here is an example:

```python
import pandas as pd

agencies_df = pd.json_normalize(data['results'])
agencies_df.head()
```

We can also get more information about the DataFrame using the `info()` method:

```python
agencies_df.info()
```

### Recipient Award Spending

In order to further dig into the spending of a particular agency and its recipients, we can use the `GET /api/v2/award_spending/recipient/` endpoint by providing the `awarding_agency_id` and `fiscal_year` as parameters.

For example, if we want to get the spending of the U.S. Agency for International Development (USAID) in the fiscal year 2024, we can use the following Python code:

```python
import requests

url = 'https://api.usaspending.gov/api/v2/award_spending/recipient/'
params = {'awarding_agency_id': agencies_df[agencies_df['toptier_agency.abbreviation'] == 'USAID']['id'].values[0], 'fiscal_year': 2024}
response = requests.get(url, params=params)
data = response.json()
print(data)
```

A successful response will return a JSON object with a `results` key containing a list of recipients that received awards from the specified awarding agency in the specified fiscal year. Each recipient object will contain information such as the recipient name, ID, and total award amount.

```json
{
    "page_metadata": {
        "count": 3398,
        "page": 1,
        "has_next_page": true,
        "has_previous_page": false,
        "next": "https://api.usaspending.gov/api/v2/award_spending/recipient/?awarding_agency_id=801&fiscal_year=2024&limit=100&page=2",
        "current": "https://api.usaspending.gov/api/v2/award_spending/recipient/?awarding_agency_id=801&fiscal_year=2024&limit=100&page=1",
        "previous": null
    },
    "results": [
        {
            "award_category": "direct payment",
            "obligated_amount": "3976708616.00",
            "recipient": {
                "recipient_name": "INTERNATIONAL BANK FOR RECONSTRUCTION AND DEVELOPMENT"
            }
        },
        {
            "award_category": "grant",
            "obligated_amount": "3066006683.00",
            "recipient": {
                "recipient_name": "UNITED NATIONS WORLD FOOD PROGRAMME"
            }
        },
        {
            "award_category": "grant",
            "obligated_amount": "2326796688.00",
            "recipient": {
                "recipient_name": "THE GLOBAL FUND TO FIGHT AIDS, TUBERCULOSIS AND MALARIA (THE GLOBAL FUND)"
            }
        },
        {
            "award_category": "grant",
            "obligated_amount": "1782258737.00",
            "recipient": {
                "recipient_name": "MISCELLANEOUS FOREIGN AWARDEES"
            }
        },
        {
            "award_category": "contract",
            "obligated_amount": "1510341410.72",
            "recipient": {
                "recipient_name": "CHEMONICS INTERNATIONAL, INC."
            }
        },
        {
            "award_category": "contract",
            "obligated_amount": "1169230053.69",
            "recipient": {
                "recipient_name": "DOMESTIC AWARDEES (UNDISCLOSED)"
            }
        },
        ...
    ]
}
```

You could take a look at the full documentation of this API endpoint [here](https://github.com/fedspendingtransparency/usaspending-api/blob/master/usaspending_api/api_contracts/contracts/v2/award_spending/recipient.md).

#### Data cleaning and conversion

After getting the result JSON, it's good to convert it to a Pandas DataFrame to select and analyze the data more easily. Here is an example:

```python
usaid_df = pd.json_normalize(data['results'])
usaid_df.head()
```

Don't forget to check the information of the DataFrame using the `info()` method:

```python
usaid_df.info()
```

#### Getting all pages of the result

To get all pages of the result, you can use the `page_metadata` key in the response JSON. If the `has_next_page` key is `true`, you can get the next page of the result by making a GET request to the `next` URL. Here is an example:

```python
while data['page_metadata']['has_next_page']:
    response = requests.get(data['page_metadata']['next'])
    data = response.json()
    usaid_df = pd.concat([usaid_df, pd.json_normalize(data['results'])], ignore_index=True)

usaid_df.info()
```

#### Data cleaning and conversion

After getting all the results, performing data cleaning is also important to make sure the data is in the right format. For example, you can convert the `obligated_amount` column to a numeric type:

```python
usaid_df['obligated_amount'] = pd.to_numeric(usaid_df['obligated_amount'])
usaid_df.info()
```


### Search for recipients data

Recipients are any entity that has received federal money in the form of contracts, grants, loans, or other financial assistance. After getting the awarding agency data, we can further search for recipients that have received awards from a specific awarding agency. To search for recipients with text, we can use the `POST /api/v2/autocomplete/recipient/` endpoint. Here is an example using Python:

```python
import requests

url = 'https://api.usaspending.gov/api/v2/autocomplete/recipient/'
data = {'search_text': 'DOMESTIC AWARDEES (UNDISCLOSED)', 'limit': 1000}
response = requests.post(url, json=data)
data = response.json()
print(data)
```

You could take a look at the full documentation of this API endpoint [here](https://github.com/fedspendingtransparency/usaspending-api/blob/master/usaspending_api/api_contracts/contracts/v2/autocomplete/recipient.md).

### Agencies Financial Balances

The USAspending API also provides information on the financial balances of federal agencies. This includes data on the budget authority, obligations, and outlays of federal agencies. To retrieve financial balances data for a specific fiscal year, we can use the `GET api/v2/financial_balances/agencies/` endpoint. Here is an example using Python:

```python
import requests

url = 'https://api.usaspending.gov/api/v2/financial_balances/agencies/'
params = {'fiscal_year': 2024, 'funding_agency_id': agencies_df[agencies_df['toptier_agency.abbreviation'] == 'USAID']['id'].values[0]}
response = requests.get(url, params=params)
data = response.json()
print(data)
```

A successful response will return a JSON object with a `results` key containing the financial balances data for the specified awarding agency in the specified fiscal year. The financial balances data will include information such as the budget authority, obligations, and outlays of the agency.

```json
{
    "results": [
        {
            "budget_authority_amount": "0.00",
            "obligation_amount": "0.00",
            "outlay_amount": "0.00",
        }
    ]
}
```

To retrieve financial balances data for all agencies in a specific fiscal year, we can loop through all awarding agencies and get the financial balances data for each agency. Here is an example:

```python
financial_balances = []

for agency_id in agencies_df['id']:
    params = {'fiscal_year': 2024, 'funding_agency_id': agency_id}
    response = requests.get(url, params=params)
    data = response.json()
    if len(data['results']) == 0:
        continue
    data['results'][0]['funding_agency_id'] = agency_id
    financial_balances += data['results']

financial_balances_df = pd.json_normalize(financial_balances)
financial_balances_df.head()
```

You may take a look at the full documentation of this API endpoint [here](https://github.com/fedspendingtransparency/usaspending-api/blob/master/usaspending_api/api_contracts/contracts/v2/financial_balances/agencies.md)