# eBay Web Scraping and Data Summary

This project aims at gathering product and sellers' data from eBay through web scraping, and then storing the data into database for further analysis, such as product comparison. Data collected from eBay includes `seller_name`, `seller_score`, `item_price`, `quantity_sold`, `best_offer_available`, `title`, `returns_allowed`, `shipping_computed_using_zip`, `condition` and `sponsor`. 

## Web scraping eBay using BeautifulSoup
BeautifulSoup is a python package for parsing html, which is useful for web scraping. After using BeautifulSoup to parse the html data, we can navigate, search and extract the data from those html elements that we are interested in.

```python
for url in non_sponsored_url:
    html_content = urllib.request.urlopen(url).read()
    soup = BeautifulSoup(html_content,'html.parser')
    
    # find all non-sponsorod products' id
    prodDetail = soup.find('div', {'class': 'prodDetailSec'})
    if not prodDetail:
        continue
    prodId_txt = prodDetail.find('td', text = 'eBay Product ID (ePID)')
    prodId = prodId_txt.findNext('td').text.strip()
    
    # save the html content and name the file using product id
    save_path = './non_sponsored/'
    completeName = os.path.join(save_path, prodId + ".html")
    with open(completeName , "wb") as f:
        f.write(html_content)
        f.close()
    time.sleep(1)
```

## Connect and store data into sqlite database

```python
# connect to SQLite3 and create a database named "eBay.db"
connection = sqlite3.connect('eBay.db')
cursor = connection.cursor()
# create sql query to create table "eBay_items"
create_table = """CREATE TABLE IF NOT EXISTS eBay_items (seller_name TEXT, seller_score REAL, item_price INTEGER,
                qty_sold INTEGER, best_offer_available BLOB, title TEXT, returns_allowed BLOB, shipping_computed BLOB,
                condition TEXT, sponsor TEXT)"""
# execute sql query
cursor.execute(create_table)
# Commits the change and close the connection to the database
connection.commit()
connection.close()
```

Please refer to [ebay_web_scraping.ipynb](./eBay_web_scraping.ipynb) for more details.