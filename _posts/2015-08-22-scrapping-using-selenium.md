---
layout: post
title: Scrapping Infinitely scrolling websites Using Python Selenium
comments: True
---

Recently I had worked on the project in which I have to scrape data from websites <a href="http://www.jabong.com/">Jabong</a> ----- looks easy ....??? No, because websites like Jabong are not fixed length websites like IMDB which I have used in my previous blog, websites like Jabong automatically expand when scrolling down to the bottom and keep on expanding until you are scrolling.

So for this case, If we use <code>lxml</code> as I have used in the previous post, <code>lxml</code> will work but problem is, that it will only scrap the products which are listed on the initial page, around 52 products will be scrapped, but for scraping 20,000 products we have to use something extra.

The biggest problem is that how to scroll the page to the bottom via a python script. Some here comes <code>selenium</code> python package. Selenium Python bindings provides a simple API to write functional/acceptance tests using Selenium WebDriver. It opens a web browser and your python program will interact with it. S <code>Selenium</code> contain various browser action keys like scrolling, selecting and <code>Xpath</code>. 


To install <code>Selenium</code> python package type
<code>pip install selenium</code> in your terminal.

### Import necessary packages

{% highlight python %}

from selenium import webdriver    #open webdriver for specific browser
from selenium.webdriver.common.keys import Keys   # for necessary browser action
from selenium.webdriver.common.by import By    # For selecting html code
import time   
{% endhighlight %} 

Now you are done with imports of necessary packages, now launch web driver specific for your browser. I am using firefox in this case you can use any browser depends on selenium list of supported browser. It will open the web browser.
It will open the web browser.

{% highlight python %}
driver = webdriver.Firefox()
driver.get("http://www.jabong.com/men/clothing/polos-tshirts/")
{% endhighlight %} 

Now next thing is that you have to scroll down until to the end of the page or up to a specific number of times depends on how many products you want to scrape. I am scrolling it 50 times, In each scrolling 52 products are added so in 50 scrolling 50 X 52 = 2600, around 2600 products will scrap. And after each scroll, I used to wait for 3 sec so that HTML page can load properly.

{% highlight python %}
for i in range(0,50):
	driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
	time.sleep(3)
{% endhighlight %}

After doing this we are having an HTML content for all the products. Now the thing required is to scrap the necessary info from a page, so for our case we are scraping product-URL for each product. so for this, we are using a <code>xpath</code> selector, If in case you want to learn about<code>xpath</code> <a href="http://www.w3schools.com/xsl/xpath_intro.asp">click here.</a>

{% highlight python %}
url =  num[0].get_attribute("data-url")
{% endhighlight %}

###Writing whole code in once.

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

For any typos or any problem feel free to comment and mail.
Till then Peace :)