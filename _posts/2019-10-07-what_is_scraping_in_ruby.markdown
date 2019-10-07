---
layout: post
title:      "What is Scraping in ruby?"
date:       2019-10-07 17:18:48 +0000
permalink:  what_is_scraping_in_ruby
---


When you start doing labs with scraping it can be confusing. I'm sure your thinking but Mike, no its not and sure the concept is easy to get behind. You want information off a website and then you go get it. Easy done. But the devil is in the details so im going to talk about the common pitfalls of scraping.


# What do we need to Scrape?
First we need a couple of things to start. I want to start broad and then work my way to code 


1.   A URL to scrape.

2.    Things we want on that page to scrape.



So far so good seams easy enough.

before you get started make sure that what your scraping doesnt have alot of Javascript a quick DM to your tech coach and they should be able to guide you.


Lets tackle the first, the URL
you are going set a varible equil to your URL

```
doc = ("https://www.thegentlemansjournal.com/five-worst-financial-crashes-time/")
```
If you have been following along with the lessons in the SWE fulltime course before this project you would have come across Nologiri. The keen among you would notice that I am missing 2 things. 
The
```
Nokogiri::HTML
```

and im not opening the URL !  so the final doc varible would look like :

```
doc = Nokogiri::HTML(open("https://www.thegentlemansjournal.com/five-worst-financial-crashes-time/"))
```
Now lets tackle number two. 

# What on the page do we want to scrape?

Here is where we want the attributes that we can call on later. So we are going to have to set a reader and a writer.
instead writing it both times we can just do an `attr_accessor``!!!!!!!! HU-RAY!!!!!!!!!!!!!!
and just put in what you wanna use .(in my case I put name, blurb and url so it looks like this ) 

```
attr_accessor :name, :blurb, :url
```

#How to get the values your looking for?

*********use pry for the next steps****************

First set a varible to what your trying to look for and then use your attribute .
```
crash_1.name 
```
Remember that scraping to find you infromation is like an onion. In the beginning  we wanna go one layer at a time. 
```
doc.css("div")
```

alright were getting closer but we need to go one layer deeper 

```
doc.css("div p ")
```
EVEN CLOSER but we still need one more layer 

```
doc.css("div p strong")
```
there we go lets pass a block to get that text using .map 

```
doc.css("div p strong").map {|title|title.text}
```
OOOOOOOOOOOOOOOOOO NOOOOOOOOOOOOOOOOO!!!! we got an array lets see if we can get the position 

```
doc.css("div p strong").map {|title|title.text}[0]
```
BOOM! title 


I did the same procedure with blurb and then put url after that. REMEMBER  BREATHE and you will get it done!!!

