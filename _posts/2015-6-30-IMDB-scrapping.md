---
layout: post
title: Web scraping from IMDB using Python
comments: True
---

Recently I worked on the project requiring me to scrap top 10 movies names from <a href="http://www.imdb.com/boxoffice/">IMDB/boxoffice</a> and the name of characters in each movie.

I have used <a href="http://lxml.de/index.html#documentation">lxml </a> package because it supports <a href="http://www.w3schools.com/xpath/">xpath</a>. In case you don't know about Xpath, you can easily learn from <a href="http://www.w3schools.com/xpath/">here.</a> You can use other packages too. The basic idea of this blog is to make approach as simple as possible.

{% highlight python %}
from lxml import html
import requests

def box_office(n):  # "n" is top n movies on the page remember "n" should always less than 10, as there is only 10 movies listed on http://www.imdb.com/boxoffice/ 

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

The above code will return two lists for box_office(5):
movie names:<code><mark>['Jurassic World', 'Inside Out', 'Terminator Genisys', 'Magic Mike XXL', 'Ted 2']</mark></code>
url of movies: <code><mark>['http://www.imdb.com/title/tt0369610/', 'http://www.imdb.com/title/tt2096673/', 'http://www.imdb.com/title/tt1340138/', 'http://www.imdb.com/title/tt2268016/', 'http://www.imdb.com/title/tt2637276/']
</mark></code>

### Scrap cast name from above url.

{% highlight python %}
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

The above code will going to return lists of names of cast in the movie.

for <code><mark>cast("http://www.imdb.com/title/tt0369610/")</mark></code> it will return:
<code><mark>['Chris Pratt', 'Bryce Dallas Howard', 'Irrfan Khan', "Vincent D'Onofrio", 'Ty Simpkins', 'Nick Robinson', 'Jake Johnson', 'Omar Sy', 'BD Wong', 'Judy Greer', 'Lauren Lapkus', 'Brian Tee', 'Katie McGrath', 'Andy Buckley', 'Eric Edelstein']
</mark></code>

Combining above piece of code into a final working program.
{% highlight python %}
from lxml import html
import requests



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

Now call <code><mark>box_office()</mark></code>  it will return two lists, one containing movie name on Boxoffice page and another containing name of a cast of that movie.

comment below for any type of typo, or suggestion.