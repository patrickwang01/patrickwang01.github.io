---
layout: post
title: "Blog Post 3 - Web Scraping"
---

In this tutorial, I will show you how to setup a webscraper with `scrapy` and use it to extract specific information from a website!


## Introduction

For my example, I will be creating a webscraper that scrapes through a movie or TV show's IMDB page and extracts the names of all of the actors and their own movies or shows they casted in themselves. One of my favorite TV show's is ***Outer Banks***, so I will using this as my example. After building the webscraper, we will make recommendations based on the results of our scraper!

Here's the link to the GitHub repository that this tutorial is based off of:



## 1. Project Setup

Before we build our webscraper, we should be aware of the pages that our scraper will navigate through. 

We will start on the main IMDB page of ***Outer Banks***:

```python
https://www.imdb.com/title/tt10293938/
```

From here, we want to navigate to the *Cast & Crew* tab:

```python
https://www.imdb.com/title/tt10293938/fullcredits/
```

Afterwards, we want to scroll down to the *Series Cast* list and click onto any actor:

```python
https://www.imdb.com/name/nm6795924/
```

As an example, this link is for Chase Stokes and if we scroll down to *Filmography*, we see all of the movies and shows that he has worked on!

This is the path that our webscraper will follow and at an actor's individual page, it will extract all of the names of the movies or shows they worked on. 

Now, we are ready to initialize our project. If we run the following commands:

```python
scrapy startproject IMDB_scraper
cd IMDB_scraper
```
We get a lot of `.py` files and a folder called `spiders`, which we will write our spiders in. 


## 2. Designing the Webscraper

To begin, we will create a class object that inherits the `Spider` component from the package `scrapy`. 

Running the code block below:
```python
import scrapy

class ImdbSpider(scrapy.Spider):
    name = 'imdb_spider'

    start_urls = ['https://www.imdb.com/title/tt10293938/']
```
We create a class object called `ImdbSpider` and set it's name as `imdb_spider`. When we want to run the spider for testing purposes, we will use this name in the command. Afterwards, we set the `start_url` as the url for the main IMDB page of our show. This will set the starting page of which our spider will begin to navigate.

## First Parse Function 

For our spider to crawl through the website, we will need to write `parse` functions. Our first parse function will navigate the spider to the *Cast & Crew* tab and then `click` on it to move onto that page. 

Let's run the code block below:
```python
def parse(self, response):
    '''
    Function that parses the main site of the movie/tv-show and extracts the href of the 'Cast and Crews' tab. 
    Yields the url for the 'Cast and Crews' site and calls parse_full_credits afterwards
    '''

    # get href portion of 'cast and crew' item
    cast = response.css("li.ipc-inline-list__item a")[2].attrib["href"]

    # join href with main url 
    cast_crew = response.urljoin(cast)

    yield scrapy.Request(cast_crew, callback = self.parse_full_credits)
```

We see that this function extracts the `href` or url from the *Cast & Crew* tab and then combines that url with the main IMDB page url. We can find these exact links and information by using the `Developer Tools` on your browser as well as `scrapy shell <url of page>`. The `Developer Tool` enables us to inspect all the individuals elements of the HTML code and `scrapy shell` does the same thing, but within the terminal! 

Going back to the function, we see that the output is a `scrapy.Request`. This command takes in our `cast_crew` variable -- which is the entire url for the *Cast & Crew* page -- and calls the next parse function that we will create. In other words, it will move our webscraper to the next page and then activate our next function!

## Second Parse Function 

Now that our scraper is on the *Cast & Crew* page, we need to write a function that enables it to "click" onto each individual actor in the show.

Using the `Developer Tools` and `scrapy shell` to find the specific HTML elements we want, if we run the code block below:

```python
def parse_full_credits(self, response):
    '''
    Function that parse the 'Cast and Crews' page and extracts the href suffix of each actor. 
    Add href suffix of each actor onto main IMDB url 
    and yields full url for each individual actor and calls parse_actor_page afterwards
    '''

    # get href portion for each actor thumbnail
    actor_names = [a.attrib["href"] for a in response.css("td.primary_photo a")]

    # set main website link
    main = "https://imdb.com"

    # combine main website url with actor href 
    actor_links = [main + actor for actor in actor_names]

    # loop through entire list of actor urls
    for link in actor_links:
        yield scrapy.Request(link, callback = self.parse_actor_page)
```
We get a function that extracts the actor's url and combines it with the main IMDB site -- not our show's main page -- to create the whole url for an individual actor! We use a list comprehension to loop through each actor on the page and store their urls in the list variable `actor_names`. Afterwards, we combine all of the individual actor urls with the main url to get a list of every actor's individual link and store it in the variable `actor_links`. Lastly, we loop through all of the links in this list and for each individual link, we yield a `scrapy.Request` that travels to that page and calls the last parse function!

## Third Parse Function

Now that our scraper is on an individual actor's webpage, we need to write a function that allows it to scrape all of the names of the movies or shows they worked on!

Using `Developer Tools` and `scrapy shell` to find the appropriate HTML elements, if we run the code block below:

```python
def parse_actor_page(self, response):
    '''
    Function that parses an individual actor's page. Extracts the name of the actor and 
    then all of the names of each movie/tv show that they worked on. 
    Yields a dictionary per movie/tv show with 2 key-value pairs of 
    {"actor": actor name, "movie_or_TV_name": movie_or_TV_name}
    '''
        
    # extract actor name 
    actor_name = response.css("span.itemprop::text").get() 
 
    rows = response.css("div.filmo-row")

    # loop through all the movies/tv shows
    for row in rows:
            
        # obtain movie/tv show name
        movie_or_TV_name = row.css("b a::text").get()

        # create dictionary with actor name and movie/tv show name
        yield {
            "actor": actor_name,
            "movie_or_TV_name": movie_or_TV_name
            }
```

Our final output is a dictionary for each individual movie/show with two key value pairs of the form `{"actor": actor name, "movie_or_TV_name": movie_or_TV_name}`. We first extract the actor's name and then the rows that each show is on. Afterwards, we loop through every row and extract the name of the movie/show and then yield a dictionary with the actor's name and the name of the specific movie/show. 

With this last parsing function, we have completed our entire spider and can run the spider to test whether it works!

To run the spider, we write the following command:
```python
scrapy crawl imdb_spider -o results.csv
```

This will run our spider starting at the main site of the show and then, it will navigate through to the other pages that our parsing function created! In the end, it should create a `csv` file called `results.csv` with two columns: the actor's name and the name of a movie/show they worked on! 


## Recommendations 





