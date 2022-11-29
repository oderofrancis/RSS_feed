# How to read an RSS feed in Python

RSS feeds have been a mainstay on the web for over 20 years now. These XML-based documents are generated by web servers and designed to be read in RSS feed readers to allow readers to be kept up-to-date with any new posts added to the feed, without the need for the user to visit the site. They’re a great way to keep up with content across numerous sites.

From a data science perspective, RSS feeds also represent a great way to gain quick and easy access to structured data on a site’s editorial content, such as blog posts or articles. In this project, I’ll show you how you can read an RSS feed in Python using web scraping. We’ll handle everything step-by-step, from scraping the RSS source code, to parsing the RSS feed contents, and outputting the text into a Pandas dataframe. Let’s get started.

## Load the packages

For this project we’ll be using three Python packages: Requests, Requests-HTML, and Pandas. Requests is one of the most popular Python libraries and is used for making HTTP requests to servers to fetch data. Requests-HTML is a web scraping library that combines Requests with the Beautiful Soup parsing package, while Pandas is used for data storage and manipulation.

Open a Jupyter notebook and import the below packages and modules. You may need to install requests_html, but you’ll usually have requests and pandas pre-installed. You can do that by entering a pip3 install package-name command in your code cell or terminal.

`!pip3 install requests_html`

## Scrape the RSS feed

The first step of reading an RSS feed in Python requires us to fetch the source of the feed itself. We can do this using the HTMLSession() feature of requests_html. The try except block creates a session, then fetches the URL, and returns the source code of the page in its response object. If it fails, we’ll use requests to throw an exception telling us why. We’ll wrap this code in a function that we can re-use in any other web scraping projects we tackle.

## Parse the RSS feed contents

Next we’ll build the Python RSS feed parser. Before we start, we first need to examine the XML elements within the code of the feed itself. RSS feeds come in various dialects, which you can determine by reading the XML declaration on the first line of the file. Mine is written in the 2005 version of Atom: <rss xmlns:atom="http://www.w3.org/2005/Atom" version="2.0">.

The Python RSS parser we build next needs to specifically detect certain elements from within the feed, which can have slightly different formats depending on the RSS dialect used. Each article in the feed is usually wrapped in an ,<item> element. Mine contains the below data, which our RSS parser needs to detect and extract.

Now we know what XML tags we need to extract, we can build the RSS parser itself. To do this, I’ve created a function called get_feed() which takes the URL of the RSS feed and passes it to the get_source() function we created above, returning the raw XML code of the feed itself in an element called response.

We then create a Pandas dataframe in which to store the parsed data, then loop through each item element found within the response. When each item is detected, we then use the find() function to look for the element names, i.e. title, pubDate, link, and description, and we extract the text from within by appending the .text argument.

Finally, we add the contents of each item to a dictionary called row which maps the data to our dataframe, and then uses the Pandas append() function to add the row of data to the dataframe, without adding an index. At the end, we return a dataframe containing all the feed contents we’ve scraped.

## Put it all together

The final step is to run our get_feed() function. We’ll pass this the address of my RSS feed (which is found at /feed.xml) and the function will fetch the feed, scrape the RSS code, parse the contents, and write the output to a Pandas dataframe. We can then examine or use that dataframe as we wish, or write its output to a range of other formats, including CSV or SQL.
