---
layout: post
title: Scrapping Infinitely scrolling websites Using Pthon Selenium
comments: True
---

Recently I had work on the project in which I have to scrap data from websites <a href="http://www.jabong.com/">Jabong</a> ----- looks easy ....??? No because websites like Jabong are not fixed length websites like IMDB which I have use in my blog, websites like jabong automatically expand when scroll down to bottom and keem on expanding untill all products in category get over.

So for this case, If we use <code>lxml</code> as I have used in the previous post, <code>lxml</code> will work but problem is, that it will only scrap the products which are listed on the initial page, around 52 products will be scrapped, but for scraping 20,000 products we have to use some thing extra.

The biggest problem is that how to get scroll the page to bottom. Some here comes <code>selenium</code> python package. Selenium Python bindings provides a simple API to write functional/acceptance tests using Selenium WebDriver. It open a web browser and your python program will interact with it. <code>Selenium</code> contain various browser action keys like scrolling, selecting and <code>Xpath</code>. 


To install <code>Selenium</code> python package type
<code>pip install selenium</code> in your terminal.

### Import necessary packages

{% highlight python %}

from selenium import webdriver    #open webdriver for specific browser
from selenium.webdriver.common.keys import Keys   # for necessary browser action
from selenium.webdriver.common.by import By    # For selecting html code
import time   
{% endhighlight %} 

Now you done with imports of necessary packages, now launch webdriver for specific for your browser. I am using firefox in this case you can use any browser depends on selenium list of supported browser.
It will open the web browser.

{% highlight python %}
driver = webdriver.Firefox()
driver.get("http://www.jabong.com/men/clothing/polos-tshirts/")
{% endhighlight %} 

Now next thing is that you have to scroll down untill to the end of the page or upto specific number of times depends on how much products you want. I am scrolling 50 times, In each scrolling 52 products are added so for 50 X 52 = 2600 , around 2600 products will scrap. And after each scroll I used to wait for 3 sec so that html page load properly.

{% highlight python %}
for i in range(0,50):
	driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
	time.sleep(3)
{% endhighlight %}

After doing this we are having html content for all the products. Now the thing required is to scrap the necessary info from page, so for our case we are scraping product-url for each product. so for this we are using <code>xpath</code> selector, If in case you want learn about <code>xpath</code> <a href="http://www.w3schools.com/xsl/xpath_intro.asp">click here.</a>

{% highlight python %}
url =  num[0].get_attribute("data-url")
{% endhighlight %}

###writing whole code in once.

{% highlight python %}
from selenium import webdriver    #open webdriver for specific browser
from selenium.webdriver.common.keys import Keys   # for necessary browser action
from selenium.webdriver.common.by import By    # For selecting html code
import time   

driver = webdriver.Firefox()
driver.get("http://www.jabong.com/men/clothing/polos-tshirts/")

def url_scrp(range=5):
	for i in range(0,range):
		driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
		time.sleep(3)

	url = driver.find_elements(By.XPATH,'//ul[@id="productsCatalog"]/li')
	return url

{% endhighlight %}

For any typos and any problem feel free to comment and and mail :)