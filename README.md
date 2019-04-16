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

Instead we will be focused on interacting with the web in some non-standard
ways, like how to programatically fetch a web site, and we will build on this to develop
more advanced tools like website mapping and brute-force tooling.  These skills
will provide insight as to what is going on in a given website so that you may
target more specific vulnerabilities later (SQL injections, etc.).

### Resources
* [`/code`](../master/code/)
* Your Parrot OS VM
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
gives us some additonal flexibilty in making our request from the web server.
Here we have specified that our python script will appear to be the
[Googlebot](https://en.wikipedia.org/wiki/Googlebot).

##### Mapping web applications
With the above you should now be ready to investigate [`/code/web_app_mapper.py`](../blob/master/code/web_app_mapper.py).
For this we are going to need a local copy of standard file and folder structure
for the given site that we want to scan.  This is easily achievable for most
sites because many of them are built on one of the following platforms called Content Management
Systems or CMS:
* [Drupal](https://www.drupal.org/download)
* [Wordpress](https://wordpress.org/download/)
* [Joomla](https://downloads.joomla.org/us/)
* [Here is a list of several other open source CMS
  tools](https://itsfoss.com/open-source-cms/)

Most web applications and web sites are created by downloading a CMS like the
ones listed above and installing them on a server (typically this server would
be running what is commonly called a LAMP stack, meaning the OS is Linux, and necessary 
service running are Apache, Mysql, and PHP).  Most websites then customize the
default CMS download with various custom scripts, themes, and plugins.  Our 
`web_app_mapper.py` will use require a locally downloaded CMS from above and a 
target URL running the specified CMS.  `web_app_mapper.py` will attempt to 
traverse the remote Url looking for files that are accessible.

1. Investigate [https://dase.cs.wright.edu/](https://dase.cs.wright.edu/), what
   CMS is it using (right click and view page source and page info).
2. Download a local copy of the CMS used by https://dase.cs.wright.edu/ (no
   response necessary).
3. Modify [`web_app_mapper.py`](../master/code/web_app_mapper.py) as necessary 
   to map [https://dase.cs.wright.edu/](https://dase.cs.wright.edu/).  
   Copy paste your edited lines into your `answers.txt`.
4. What does the output of `web_app_mapper.py` tell you about the site?  How is
   this information useful?
5. What might be some limitations of this approach to mapping web applications?

### Task 2 - What's in a name
Similar to the crackstation wordlist for passwords, there exist wordlists
designed solely to assist in finding hidden resources in web applications.

1. Download [SVNDigger](https://www.netsparker.com/s/research/SVNDigger.zip).
   How many words are in `all.txt`?  What are they? (summarize please...)
2. Would a worklist such as SVN Digger for web applications be more or less 
   useful than a wordlist like crackstation for passwords?  Why or why not?

### Task 3 - Forced browsing
The above `web_app_mapper.py` is really useful *IF* we know what CMS is running
on the host, but if we do not know what CMS is being used we will be missing a
lot of accessible files on the remost host.  

To discover some more (hopefully juicy) breadcrumbs that may exist on the host
we will need a way to scan for files other than those that make up the CMS.  For
this purpose we will be using [`/code/content_bruter.py`](../master/code/content_bruter.py).

1. Modify `content_bruter.py` to scan the site [http://testphp.vulnweb.com](http://testphp.vulnweb.com)
   using the wordlist `all.txt` downloaded from SVNDigger.  What output do you
   see?  How is this output useful for vulnerability testing?
2. What sort of challenges would we face using the previous `web_app_mapper.py` 
   on [http://testphp.vulnweb.com](http://testphp.vulnweb.com)?
3. Would `content_bruter.py` find anything different than `web_app_mapper.py`
   on a site like [https://dase.cs.wright.edu/](https://dase.cs.wright.edu/)?
   Why or why not?


### Task 4 - Brute Force HTML Form Authentication



### Acknowledgement
Portions of this lab were derived from [Black Hat Python: Python Programming for
Hackers and Pentesters, by Justin Seitz](https://nostarch.com/blackhatpython).
