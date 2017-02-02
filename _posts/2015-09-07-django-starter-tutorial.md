---
layout: post
title: Django Tutorial for starters
comments: True
---

Hi guys, I am writing this blog to help people starting with Django. This tutorial inclined on basic of Django so that any one cat start with. If anyone has some other doubt than free to ask me by making a comment or email me. 
<br>
Django is python framework for web development, Before Jango I only know <a href="http://flask.pocoo.org/">Flask</a>(Other python Framework), it is very much similar to a simple python programming. For learning Flask, you only have to read very first page of there documentation And you ready to build. whereas Django is the vast and versatile framework. It is very nicely written so to make your code look cleaner. 
<br>

<h4>Start with basic installation</h4>
<br>
Install using pip:
<br>
<code style="background-color: #d3d3d3">pip install django</code>
<br>
confirm install by typing:
<br>
<code style="background-color: #d3d3d3">django-admin --version</code>
<br>
<code style="background-color: #d3d3d3">1.9</code>
<br>
<b>No error uptill here, means you have succefully install django</b>
<br>
Make directory <code style="background-color: #d3d3d3">django_project</code> by typing command:
<br> <code style="background-color: #d3d3d3">mkdir django_project</code>
<br>
Now let's do some Django. 
<br>
<h4>Creating a project:</h4>
<br>
From command line cd into directory and run following command:
<code style="background-color: #d3d3d3">django-admin startproject mysite</code>
<br>
This will create Django project name "mysite". 
<br>
Let’s look at what startproject created:
<br>
<code style="background-color: ">
mysite/<br>
&nbsp;&nbsp;&nbsp;&nbsp;manage.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;mysite/<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__init__.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;settings.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;urls.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wsgi.py<br></code>

<ul>
  <li>The outer mysite/ root directory is just a container for your project.</li>
  <li ><code>manage.py</code> is a command line utility that interacts with Django project in various ways.</li>
  <li>The inner mysite/ directory is the actual Python package for your project.</li>
  <li>mysite/__init__.py: An empty file that tells Python that this directory should be considered a Python package. </li>
  <li>mysite/settings.py: Settings/configuration for this Django project. It contains configuration related to database setup, template static file location etc.</li>
  <li>mysite/urls.py: The URL declarations for this Django project; </li>
</ul>

<br>
<h4>Development server</h4>
<br>
<code style="background-color: ">python manage.py runserver</code>
<br>
In your web browser type <code>http://127.0.0.1:8000/</code>. You will probably be going to see the nice light blue window showing a success message.  <br>
Django comes with lightweight development server so that you can boost development of your application. But we suggest not to used for production. 
<br><b>Congratulate guys you have successfully installed Django</b>

<br>
Now next part is adding application and basic functioning of files present.
<br>
<h4>Creating polls app:</h4><br>
For creating your first app type command:<br>
<code style="background-color: ">python manage.py startapp polls</code><br>

This command will generate polls directory with follwing file:<br>

polls/<br>
&nbsp;&nbsp;&nbsp;&nbsp;__init__.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;admin.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;apps.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;migrations/<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__init__.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;models.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;tests.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;views.py<br>

<h4>Writing views</h4>
<br>
"Views.py" as by name is responsible what user will view. Views are written in file views.py .<br>
<code>polls/views.py</code><br>

{% highlight python %}

from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world. You're at the polls index.") 
{% endhighlight %} 

This is simplest view in Django. Views are called when they map with URL - for this we need to URLconf.
<br>
To create a URLconf in the polls directory, create a file called urls.py. Your app directory will look like:
<br>

polls/<br>
&nbsp;&nbsp;&nbsp;&nbsp;__init__.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;admin.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;apps.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;migrations/<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__init__.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;models.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;tests.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;urls.py<br>
&nbsp;&nbsp;&nbsp;&nbsp;views.py<br>

In the polls/urls.py file include the following code:
<br>
<code>polls/urls.py</code><br>

{% highlight python %}
from django.conf.urls import url
from . import views
urlpatterns = [
    url(r'^$', views.index, name='index'),
]
{% endhighlight %} 

The next step is to point the root URLconf at the <code>polls.urls</code> module. In,  <code>mysite./urls.py</code>, add an import for django.conf.urls.include and insert an include() in the "urlpatterns" list, so you have:
<code>mysite/urls.py</code>
<br>
{% highlight python %}
from django.conf.urls import include, url
from django.contrib import admin
urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
{% endhighlight %} 

Now again runserver, And go to link <a href="http://localhost:8000/polls/"> http://localhost:8000/polls/</a> in your browser.
<br>

you are done with basic django installation now for more learning got <a href="https://docs.djangoproject.com/en/1.9/intro/tutorial02/">Django Official tutorial part 2</a>.
<br>
For any suggestion bugs and doubt feel free to comment or email.
<br>