# Scrapy Tutorial

## Installing Scrapy
Before using, remember to create a new virtual environment

```
python -m venv [name]
```

To activate the virtual environment you need to run another command:
```
source [name]/bin/activate
```
After this, just

```
pip install scrapy
```

## Starting and setting up a basic project

- Starting the project itself (important to remember to do this in the same level as the virtual environment)

```
scrapy startproject [project-name]
```
![Pasted image 20250205173543](https://github.com/user-attachments/assets/75fd988a-7e98-43f0-9446-278069b44fa4)


- Create a spider:
```
scrapy genspider spider-name website
```

- You can see all your spiders with:
```
scrapy list
```

- Scrappy has a shell of its own. It can run some methods directly in the command line, just like python can do.
```
scrapy shell
```

- In the shell, it's nice to do a first fetch, just to see if everything is 200, for that just:
```scrapy-shell
fetch('your-url')
```

- In the tutorial example, we found the product in the source code
![Pasted image 20250205175345](https://github.com/user-attachments/assets/8b314485-1153-441d-b7e4-6f5899114e7e)

after this, just get the class name, so you can search for the response of the css of all items of that class:
```scrapy-shell
response.css('product-item')
```

- Now that we know this, it's possible to create a collection of all the items that correspond with that, simply by saying:
```scrapy-shell
products = response.css('product-item')
```

- In the example, we also need to get the title of the product, which is a bit more complicated. The name is inside an a tag `<a> </a>`  

- After this, to get a single product and test with it:
```scrapy-shell
>>> products = response.css('product-item')

>>> product = products[0]

>>> product
<Selector query='descendant-or-self::product-item' data='<product-item class="product-item " r...'>

>>> product.css('a.product-item-meta__title')
[<Selector query="descendant-or-self::a[@class and contains(concat(' ', normalize-space(@class), ' '), ' product-item-meta__title ')]" data='<a href="/products/2-5kg-bulk-of-our-...'>]

>>> product.css('a.product-item-meta__title').get()
'<a href="/products/2-5kg-bulk-of-our-41-milk-hot-chocolate-drops" class="product-item-meta__title">2.5kg Bulk 41% Milk Hot Chocolate Drops</a>'

>>> product.css('a.product-item-meta__title::text').get()
'2.5kg Bulk 41% Milk Hot Chocolate Drops'

```

On those tests, it is possible to see how we can extract information from within the css.

In the page, the code being analyzed was like this:
```css
<a href="/products/2-5kg-bulk-of-our-41-milk-hot-chocolate-drops" class="product-item-meta__title">2.5kg Bulk 41% Milk Hot Chocolate Drops</a>
```

- Now to get the href (for us to move to a different page if needed) it was a bit different, from here we would need to use attrib:
```scrapy-shell
product.css('a.product-item-meta__title').attrib[href]
```

You could also do it like this

```scrapy-shell
product.css('div.product-item-meta a').attrib['href']
```

this will return just what is in the href

### Spider code

```python
import scrapy

class ChocolatespiderSpider(scrapy.Spider):

	name = "chocolatespider"
	allowed_domains = ["chocolate.co.uk"]
	start_urls = ["https://chocolate.co.uk/collections/all"]

  

	def parse(self, response):
		pass
```

the parse function is what is actually ran as you get a spider to work.
