# Addition Lab: Data scraping and cleaning

---

# Overview

In this lab, you will learn how to collect data from a website and clean the data.All the code will be written in Python and run in Jupyter Notebook supported platform.

## Introduction

Data is the most important part of machine learning. The data collection and cleaning process is usually the most time-consuming part of data analysis and machine learning. In this lab, we will learn how to collect data from a website and clean the data for data analysis and machine learning.

## Objectives

- Learn how to collect data from a website
- Learn how to clean the data

## Prerequisites

- Python programming
- Jupyter Notebook
- Basic knowledge of machine learning
- Some knowledge of HTML and Javascript
- Some knowledge of JSON data format
- Some knowledge of web scraping

# Case Study: Citysuper

In this lab, we are going to collect product data from a website for training a classifier to recognize the type of product. The website is [https://online.citysuper.com.hk/](https://online.citysuper.com.hk/). Citysuper is a supermarket in Hong Kong targeting high-end customers. The website contains a lot of information about their products in different categories.

## Exploring the data source and Data collection

Let's click on [https://online.citysuper.com.hk/](https://online.citysuper.com.hk/) to see how the website looks like and what information we can get from it.

![Citysuper homepage screenshot](https://url2png.hkbu.app/1600/online.citysuper.com.hk)

There are top menus for different categories of products. We can click on the menu to see the products in the category. For example, if we mouse-over on the "Meat" menu, we can see the products sub-categories under "Meat".

Simply click on the "Beef" sub-category, we can see the products under "Beef" and the URL would be [https://online.citysuper.com.hk/collections/meat-and-poultry-beef](https://online.citysuper.com.hk/collections/meat-and-poultry-beef).

### A trick to get all the products in the website

By exploring the website, we will notice that it provides a way to search for products. For example, if we search for "beef", we can see the products related to "beef" and the URL would be [https://online.citysuper.com.hk/search?q=beef](https://online.citysuper.com.hk/search?q=beef).

We can use this trick to get all the products in the website by searching for single common characters like "a", "b", "c", etc. For example, if we search for "a", we can see the products related to "a" and the URL would be [https://online.citysuper.com.hk/pages/search-results?q=a](https://online.citysuper.com.hk/pages/search-results?q=a).

That's it! We can use this trick to get all the products in the website by looping through all the characters in the alphabet.

### Inspecting traffic to get data in JSON format

After we get the URL for all the products, we can use the trick to get the data in JSON format. For example, if we search for "1", we can see the products related to "1" and inspect the traffic. You will see a request to something like
`https://api.fastsimon.com/full_text_search?q=1&page_num=1&UUID=a576ef0e-353e-426c-8673-dbefcd7987b4`.

You may search for `full_text_search` in the `Network` tab of the `Developer Tools` in the browser. You will see a request to something like `https://api.fastsimon.com/full_text_search?q=1&page_num=1&UUID=a576ef0e-353e-426c-8673-dbefcd7987b4`. You may click on the request and see the `Response` tab. You will see the data in JSON format.

![](lab1/inspect-traffic.png)

The response is in Javascript format, and we can see the data in JSON format by removing the `ispSearchResult(` and `)` at the beginning and the end of the response.

```js
ispSearchResult({
    "uuid": "a576ef0e-353e-426c-8673-dbefcd7987b4",
    "items": [{
        "l": "HERSHEY'S Genuine Chocolate Flavor Syrup  (680g)",
        "c": "HKD",
        "u": "/products/hersheys-genuine-chocolate-flavor-syrup-15400137",
        "p": "42.00",
        "p_min": "",
        "p_max": "",
        "p_c": "0.00",
        "p_min_c": "0.00",
        "p_max_c": "0.00",
        "d": "Keep refrigerated after opening *Photo for reference only. ",
        "t": "https://cdn.shopify.com/s/files/1/2597/8324/products/15400137-1-hershey-s-genuine-chocolate-flavor-syrup-680g_large.jpg?v=1661945179",
        "t2": "https://assets.instantsearchplus.com/thumbs/cdn.shopify.com/d3b92be8-bb54-4fb1-bc48-6cf76dd02736",
        "f": 0,
        "s": "4459343970347::31771184824363",
        "sku": "15400137",
        "p_spl": 0,
        "id": "4459343970347",
        "skus": ["15400137", "HERSHEY'S", "\u6731\u53e4\u529b\u5473\u7cd6\u6f3f", "", "(680g)"],
        "v_c": 1,
        "iso": false,
        "vra": [[31771184824363, [["Barcode", ["HERSHEY'S \u6731\u53e4\u529b\u5473\u7cd6\u6f3f (680g)"]], ["Price", ["HKD:42.00"]], ["Product-sku", ["15400137"]], ["Sellable", [true]], ["Title", ["Default Title"]]]]],
        "vrc": {}
    },
    ...],
    "total_results": 136,
    "term": "a",
    "p": 6,
    "total_p": 7,
    "alternatives": [],
    "results_for": null,
    "isp_quick_view_mode": 1,
    "auto_facets": false,
    "related_results": false
});
```

## Collecting the data with Python code

After generating the JSON data, we could use Python code to collect the data and save it to a CSV file. In order to collect the data, we need to use the `requests` library to send a web request to the above URL and then parse the JSON response with `json` library.

```python
import requests
import json
import pandas as pd

r = requests.get('https://api.fastsimon.com/full_text_search?q=1&page_num=3&UUID=a576ef0e-353e-426c-8673-dbefcd7987b4')
data = json.loads(r.text.lstrip('ispSearchResult(').rstrip(');'))

data
```

It will generate the following output.

```python
{'uuid': 'a576ef0e-353e-426c-8673-dbefcd7987b4',
 'items': [{'l': 'KOLIOS Authentic Greek Yogurt - 10% Fat  (1kg)',
   'c': 'HKD',
   'u': '/products/kolios-authentic-greek-yogurt-10-fat-1kg',
   'p': '99.00',
   'p_min': '',
   'p_max': '',
   'p_c': '0.00',
   'p_min_c': '0.00',
   'p_max_c': '0.00',
   'd': 'Founded in 1948, the KOLIOS family has been producing its products according to well-kept recipes for many generations. Nowadays, it is considered as one of the leading companies in Greek quality cheese and yogurt production. Made from cow’s milk, this yogurt has been repeatedly awarded with gold medals for its excellent quality, has a typical thick texture and the natural, gentle and creamy taste. Perfect with breakfast cereals, or to create amazing salad dressings. \xa0 Keep refrigerated *Photo for reference only. ',
   't': 'https://cdn.shopify.com/s/files/1/2597/8324/products/301322364-1-authentic-greek-yogurt-10-fat-s_large.jpg?v=1662552499',
   't2': 'https://assets.instantsearchplus.com/thumbs/cdn.shopify.com/f50fc661-a698-48ba-8347-e92d3920d1c9',
   'f': 0,
   's': '1382480904235::32934741671979',
   'sku': '301322364',
   'p_spl': 0,
   'id': '1382480904235',
   'skus': ['301322364', 'KOLIOS', '希臘乳酪', '-', '10%脂肪', '', '(1kg)'],
   'v_c': 1,
   'iso': False,
   'vra': [[32934741671979,
     [['Barcode', ['KOLIOS 希臘乳酪 - 10%脂肪 (1kg)']],
      ['Price', ['HKD:99.00']],
      ['Product-sku', ['301322364']],
      ['Sellable', [True]],
      ['Title', ['Default Title']]]]],
   'vrc': {}},
  ...],
 'total_results': 683,
 'term': '1',
 'p': 1,
 'total_p': 35,
 'alternatives': [],
 'results_for': None,
 'isp_quick_view_mode': 1,
 'auto_facets': False,
 'related_results': False
}
```

There are data in the dictionary that meaningful for us, we may guess the meaning with they key value.

- `items` : the list of the product information
  - `l` : the product name
  - `c` : the currency
  - `u` : the product url
  - `p` : the price
  - `d` : the description
  - `t` : the image url
  - `sku` : the product sku
- `total_results` : the total number of the product
- `term` : the search term
- `p` : the current page
- `total_p` : the total pages

In other to retrieve the data, we need to write a function to loop through all pages and save the data into a list. We also import `urllib.parse` to encode the search query in URL encoding, in case it contains special characters.

```python
import requests
import json
import urllib.parse

def get_product(q, page):
    url = f'https://api.fastsimon.com/full_text_search?q={urllib.parse.quote(q)}&page_num={page}&UUID=a576ef0e-353e-426c-8673-dbefcd7987b4'
    response = requests.get(url)
    data = json.loads(response.text.lstrip('ispSearchResult(').rstrip(');'))
    return data['items'], data['total_p']

def get_all_products(q):
    page = 1
    products = []
    while True:
        data, total = get_product(q, page)
        products += data
        if page >= total:
            break
        page += 1
    return products
```

Let's try out the function by searching with several keywords. We will search for `candy`, `coffee` and `noodle`.

```python
candy = get_all_products('candy')
print(len(candy))
candy
```

```python
coffee = get_all_products('coffee')
print(len(coffee))
coffee
```

```python
noodle = get_all_products('noodle')
print(len(noodle))
noodle
```

# Data Processing and Cleaning

The data collected previously are considered as raw data, we need to process and clean them before we can use them for analysis. We will use `pandas` to process the data.

## Candy

For the candy data, we will convert the list of dictionary into a `pandas` dataframe. We can use `pd.DataFrame()` to convert the list of dictionary into a `pd.DataFrame` and it could be shown as a table format in Jupyter Notebook.

```python
import pandas as pd

candy_df = pd.DataFrame(candy)
candy_df
```

### View the Data

To see the first 5 rows of the dataframe, we can use `head()` function.

```python
candy_df.head()
```

To see the last 5 rows of the dataframe, we can use `tail()` function.

```python
candy_df.tail()
```

Let's check the data type of each column.

```python
candy_df.info()
```

```bash
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 350 entries, 0 to 349
Data columns (total 22 columns):
 #   Column   Non-Null Count  Dtype 
---  ------   --------------  ----- 
 0   l        350 non-null    object
 1   c        350 non-null    object
 2   u        350 non-null    object
 3   p        350 non-null    object
 4   p_min    350 non-null    object
 5   p_max    350 non-null    object
 6   p_c      350 non-null    object
 7   p_min_c  350 non-null    object
 8   p_max_c  350 non-null    object
 9   d        350 non-null    object
 10  t        350 non-null    object
 11  t2       350 non-null    object
 12  f        350 non-null    int64 
 13  s        350 non-null    object
 14  sku      350 non-null    object
 15  p_spl    350 non-null    int64 
 16  id       350 non-null    object
 17  skus     350 non-null    object
 18  v_c      350 non-null    int64 
 19  iso      350 non-null    bool  
 20  vra      350 non-null    object
 21  vrc      350 non-null    object
dtypes: bool(1), int64(3), object(18)
memory usage: 33.2+ KB
```

### Convert Column Data Type

The price column is in string format, thus it shows as `object` in the data type. We need to convert it into numeric format by using `astype()` function

```python
candy_df['p'] = candy_df['p'].astype(float)
```

Let's check the data type again.

```python
candy_df.info()
```

```bash
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 350 entries, 0 to 349
Data columns (total 22 columns):
 #   Column   Non-Null Count  Dtype  
---  ------   --------------  -----  
 0   l        350 non-null    object 
 1   c        350 non-null    object 
 2   u        350 non-null    object 
 3   p        350 non-null    float64
 4   p_min    350 non-null    object 
 5   p_max    350 non-null    object 
 6   p_c      350 non-null    object 
 7   p_min_c  350 non-null    object 
 8   p_max_c  350 non-null    object 
 9   d        350 non-null    object 
 10  t        350 non-null    object 
 11  t2       350 non-null    object 
 12  f        350 non-null    int64  
 13  s        350 non-null    object 
 14  sku      350 non-null    object 
 15  p_spl    350 non-null    int64  
 16  id       350 non-null    object 
 17  skus     350 non-null    object 
 18  v_c      350 non-null    int64  
 19  iso      350 non-null    bool   
 20  vra      350 non-null    object 
 21  vrc      350 non-null    object 
dtypes: bool(1), float64(1), int64(3), object(17)
memory usage: 34.6+ KB
```

We can see that the data type of the price column has been changed to `float64`. After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
candy_df.describe()
```

```bash
                  p           f  p_spl    v_c
count    350.000000  350.000000  350.0  350.0
mean      41.380000         0.0    0.0    1.0
std       59.607658         0.0    0.0    0.0
min        5.000000         0.0    0.0    1.0
25%       18.250000         0.0    0.0    1.0
50%       26.000000         0.0    0.0    1.0
75%       40.000000         0.0    0.0    1.0
max      744.000000         0.0    0.0    1.0
```

### Cleaning up unnecessary data

We could see that the data includes some Candle products, we need to remove them from the dataframe. We could do it by using `str.contains()` function to filter out the candle products and `~` to reverse the result.

```python
candy_df = candy_df[~candy_df['l'].str.lower().str.contains('candle')]
```

### Extract Data from Column

When exploring the candy data, we found that there are some useful information in the `l` column, like types of candy, brand, and weight. We can extract the information and create new columns for them.

Let's extract jelly from the `l` column and create a new dataframe with the extracted data.

```python
jelly_df = candy_df[candy_df['l'].str.lower().str.contains('jelly')]
jelly_df
```

After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
jelly_df.describe()
```

Let's extract gummy from the `l` column and create a new dataframe with the extracted data.

```python
gummy_df = candy_df[candy_df['l'].str.lower().str.contains('gummy')]
gummy_df
```

After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
gummy_df.describe()
```

## Coffee

For the coffee data, we will convert the list of dictionary into a `pandas` dataframe. We can use `pd.DataFrame()` to convert the list of dictionary into a `pd.DataFrame` and it could be shown as a table format in Jupyter Notebook.

```python
coffee_df = pd.DataFrame(coffee)
coffee_df
```

### Convert Column Data Type

The price column is in string format, thus it shows as `object` in the data type. We need to convert it into numeric format by using `astype()` function

```python
coffee_df['p'] = coffee_df['p'].astype(float)
```

After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
coffee_df.describe()
```

### Separate Tea and Coffee

After we view the data, we found that there are some tea products in the coffee data. We need to remove them from the dataframe. We could do it by using `str.contains()` function to filter out the tea products and `~` to reverse the result.

```python
tea_df = coffee_df[coffee_df['l'].str.lower().str.contains('tea')]
tea_df
```

After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
tea_df.describe()
```

We could further extract the green tea products from the dataframe.

```python
green_tea_df = tea_df[tea_df['l'].str.lower().str.contains('green tea')]
green_tea_df
```

After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
green_tea_df.describe()
```

After that we could remove the tea products from the coffee dataframe.

```python
coffee_df = coffee_df[~coffee_df['l'].str.lower().str.contains('tea')]
coffee_df
```

After that, we can use `describe()` function to see the basic statistics of the numeric columns.

```python
coffee_df.describe()
```

## Exercise 1: Noodle

Please try to process the noodle data by yourself. You should initialize a `pd.DataFrame` name as `noodle_df` with the noodle data and convert the price column into numeric format.

After that, you should extract the `udon` and `ramen` from the `l` column and create a new dataframe for each of them namely `udon_df` and `ramen_df`.

# Fetch image data with URLs from `DataFrame`

We can use `requests` to fetch the image data from the image url and save it into a file. We can use `shutil.copyfileobj` to shorten the code. You may need to create a folder named `images` in the same folder of the notebook.

```bash
!mkdir -p images
```

```python
import requests
import shutil

def download_image(row):
    dest = f'images/{row["sku"]}.jpg'
    url = row['t']
    print(f'downloading image for {row["l"]}')
    with requests.get(url, stream=True) as r:
        with open(dest, 'wb') as f:
            shutil.copyfileobj(r.raw, f)
    return row
```

For example, we can use `apply()` function to apply the `download_image()` function to each row of the dataframe on `candy_df`. It would take some time to download all the images.

```python
candy_df.apply(download_image, axis=1)
```

After that, we can use `PIL` to show the image. For example, we can show the first image in the dataframe with following code.

```python
from PIL import Image
from IPython.display import display

img = Image.open(f'images/{candy_df.iloc[0]["sku"]}.jpg')
display(img)
```

