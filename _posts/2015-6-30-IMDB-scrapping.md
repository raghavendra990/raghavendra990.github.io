---
layout: post
title: Web scrapping from IMDB using Python
comments: True
---

Recently I worked on the project requiring me to scrap top 10 movies names from <a href="http://www.imdb.com/boxoffice/">IMDB/boxoffice</a> and the name of charachters in each movie.

I have used <a href="http://lxml.de/index.html#documentation">lxml </a> package because it support <a href="http://www.w3schools.com/xpath/">xpath</a>. In case you don't know about Xpath, you can easily learn from <a href="http://www.w3schools.com/xpath/">here.</a> You can use other packages too. Basic idea of this blog is to make approach as simple as possible.

{% highlight python linenos %}
from lxml import html
import requests

def box_office(n):         			# "n" is top n movies on the page remember "n" should always less than 10, as there is only 
									# 10 movies listed on http://www.imdb.com/boxoffice/ 
	page = requests.get('http://www.imdb.com/boxoffice/')
	tree = html.fromstring(page.text)
	path = tree.xpath('//tbody/tr/td[3]/a')
	movie = []
	url = []

	for i in range(0 , n):
		link = path[i]
		movie.append(link.text)
		url.append("http://www.imdb.com"+ link.attrib.get('href'))
	return movie, url
{% endhighlight %}

The above code will return two list for box_office(5):
movie names:<code>['Jurassic World', 'Inside Out', 'Terminator Genisys', 'Magic Mike XXL', 'Ted 2']</code>
url of movies: <code>['http://www.imdb.com/title/tt0369610/', 'http://www.imdb.com/title/tt2096673/', 'http://www.imdb.com/title/tt1340138/', 'http://www.imdb.com/title/tt2268016/', 'http://www.imdb.com/title/tt2637276/']
</code>

### Scrap cast name from above url.

{% highlight python linenos %}
from lxml import html
import requests

def cast(url):
	page_url = requests.get(url)
	tree_url = html.fromstring(page_url.text)

	path_url_odd = tree_url.xpath('//tr[@class="odd"]/td[2]/a/span')
	path_url_even = tree_url.xpath('//tr[@class="even"]/td[2]/a/span')

	cast = []

	for i in range(0,len(path_url_odd)):
		try:
			cast.append(path_url_odd[i].text)
			cast.append(path_url_even[i].text)
		except IndexError:
			continue
	return cast
{% endhighlight %} 

The above code will going to return list of names of cast in the movie.

for <code>cast("http://www.imdb.com/title/tt0369610/")</code> it will return:
<code>['Chris Pratt', 'Bryce Dallas Howard', 'Irrfan Khan', "Vincent D'Onofrio", 'Ty Simpkins', 'Nick Robinson', 'Jake Johnson', 'Omar Sy', 'BD Wong', 'Judy Greer', 'Lauren Lapkus', 'Brian Tee', 'Katie McGrath', 'Andy Buckley', 'Eric Edelstein']
</code>

Combining above peice of code into final working program.
{% highlight python linenos %}
from lxml import html
import requests


###############################################################################################################
# 						Actual function which is to be call
#				NOTE: Value of "n" should be less than 10 because onle 10 movies list is there on "http://www.imdb.com/boxoffice/"
def box_office(n):
	page = requests.get('http://www.imdb.com/boxoffice/')
	tree = html.fromstring(page.text)

	path = tree.xpath('//tbody/tr/td[3]/a')

	movie = []
	movie_cast = []


	for i in range(0 , n):
		link = path[i]
		movie.append(link.text)
		movie_cast.append(cast("http://www.imdb.com"+ link.attrib.get('href')))
	return (movie , movie_cast)

###########################################################################################################
#					cast function scrab cast of particular movie, It takes url as argument

def cast(url):
	page_url = requests.get(url)
	tree_url = html.fromstring(page_url.text)

	path_url_odd = tree_url.xpath('//tr[@class="odd"]/td[2]/a/span')
	path_url_even = tree_url.xpath('//tr[@class="even"]/td[2]/a/span')

	cast = []

	for i in range(0,len(path_url_odd)):
		try:
			cast.append(path_url_odd[i].text)
			cast.append(path_url_even[i].text)
		except IndexError:
			continue
	return cast


{% endhighlight %} 

Now call <code>box_office()</code> it will return two list, one containing movie name on boxoffice page and other containing name of cast of that movie.

comment below for any type of typo, or suggestion.