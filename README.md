# Web Hackery

### Introduction
As stated in class, one of the most vulnerable parts of a network is its web
applications.  Web applications typically have a larger attack surface due to
their complexity and are frequently used to gain access.  In this lab we will be
investigating some of the most basic python tools for interacting with the web.

### Background
There are a multitube of mature applications for exploiting certain
vulnerabilities such as SQL injection, cross site scripting, PHP injection, etc.
In this lab we will not be focusing on these individual topics since there is a
good deal of outside information should you choose to pursue them.

Instead we will be focused on using python to interact with the web in some
basic ways, like how to fetch a web site, and we will build on this to develop
more advanced tools like website mapping and brute-force tooling.  These skills
will provide insight as to what is going on in a given website so that you may
target more specific vulnerabilities (SQL injections, etc.) later.

### Resources
* [`/code`](../blob/master/code/)
* [AWS educate (Login link)](https://www.awseducate.com/signin/SiteLogin)
* [AWS Build link](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=CEG-4900Lab02&templateURL=https:%2F%2Fs3.amazonaws.com%2Fwsu-cecs-cf-templates%2Fceg4900lab1.yml)

### Task 1 -
The most basic python library we will be using is [`urllib2`](https://docs.python.org/2/library/urllib2.html).  
Lets jump right in and write a simple GET request in python:
```
import urllib2

body = urllib2.urlopen("http://www.wright.edu")

print body.read()
```

Go ahead and run the above in an interactive python terminal.  Comparing this to our
much earlier `tcp-client.py` we should see that this gives us several
advantages, my personal favorite being we dont have to juggle so much text with
regards to specifically formatted GET requests.  

But this is just the beginning.  Below is a more advanced GET request that uses
the [`urllib2.Request()` class](https://docs.python.org/2/library/urllib2.html#urllib2.Request):

```
import urllib2

URL = "http://www.wright.edu"

headers = {}
headers['User-Agent'] = "Googlebot"

request = urllib2.Request(URL,headers=headers,)
response = urllib2.urlopen(request)

print response.read()
response.close()
```

The [`urllib2.Request()` class](https://docs.python.org/2/library/urllib2.html#urllib2.Request)
gives us some additonal flexibilty in making our request fro mth eweb server.
Here we have specified that our python script will appear to be the
[Googlebot](https://en.wikipedia.org/wiki/Googlebot).

##### Mapping web applications
With the above you should now be ready to investigate [`/code/web_app_mapper.py`](../blob/master/code/web_app_mapper.py).
For this we are going to need a local copy of standard file and folder structure
for the given site that we want to scan.  This is easily achievable since most
sites a built on one of the following platforms called Content Management
Systems or CMS:
* [Drupal](https://www.drupal.org/download)
* [Wordpress](https://wordpress.org/download/)
* [Joomla](https://downloads.joomla.org/us/)
* [Here is a list of several other open source CMS
  tools](https://itsfoss.com/open-source-cms/)

Most web applications and web sites are created by downloading a CMS like the
ones listed above and installing them on a server (typically this server would
be running what is called a LAMP stack, meaning the OS is Linux, and necessary 
service running are Apache, Mysql, and PHP).  Most webistes then customize the
default CMS download with various custom scripts, themes, and plugins.  Our 
`web_app_mapper.py` will use the locally downloaded CMS from above and a 
target URL running the specified CMS.  `web_app_mapper.py` will attempt to 
traverse the remote Url looking for files that are accessible.

1. Use `web_app_mapper.py`
2. some questions regarding output and usage (needs to have local copy of
   directory structure...)


### Task 2 - Brute Force Files and Directories
The above `web_app_mapper.py` is really useful *IF* we know what CMS is runnign
on the host, but if we do not know what CMS is being used, or if we expect a lot
of customized directories we will be missing a lot of accessible files on the
remost host.  To discover some more (hopefully juicy) breadcrumbs that may have
been left on the host we will need a more *brute*al tool (sorry...)

[`/code/content_bruter.py`](../blob/master/code/content_bruter.py) is a tool
that will use a wordlist of common (and hopefully useful) file names that it
will search against on the remote host.  The wordlist we will use is provided
by [SVNDigger](https://www.netsparker.com/s/research/SVNDigger.zip).

3. Use `content_bruter.py
4. Some questions regarding output (i have not gotten content bruter working
   yet)


[Scan website](http://testphp.vulnweb.com/)

### Task 3 - Brute Force HTML Form Authentication

### Task 4 - Burp Suite
BHP chapter 6


### Acknowledgement
Portions of this lab were derived from [Black Hat Python: Python Programming for
Hackers and Pentesters, by Justin Seitz](https://nostarch.com/blackhatpython).
