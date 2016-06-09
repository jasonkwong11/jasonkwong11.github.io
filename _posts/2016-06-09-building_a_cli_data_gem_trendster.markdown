---
layout: post
title:  "Building a CLI Data Gem: Trendster"
date:   2016-06-09 16:01:36 -0400
---

 This blog post describes the project and process of building a CLI gem that provides a CLI interface to an external data source. My gem, Trendster, provides a CLI interface to Cuyahoga County Public Library's event page. It offers the user details of the most current twenty events at the library's 28 branches across northeast Ohio.
 
 Here's a quick summary of how it works: it scrapes the library's index page for the path to each event's dedicated page. It creates new URL's from those scraped paths and creates Nokogiri HTML docs from the new URLs of each event's page. From each event's Nokogiri doc, I scraped the doc for the event's name, description, date/time, location, and intended audience. These values were stored in a hash, which was used to instantiate new Event objects. The CLI method calls on these Event objects to puts their name, description, date/time, location, and intended audience to the user after first offering a list of events for the user to choose from. The application accounts for invalid user inputs and allows the user see the list of events again and exit the application.

	The scope of this gem evolved through multiple iterations. First it was supposed to scrape trending topics from multiple social media websites including Facebook, Twitter, Youtube, and Google Trends. I quickly realized how that might be biting off more than I could chew. Then I narrowed the scope to on trending topics on Twitter. I realized that there were already many gems on Github that accomplished the same thing I set out to do so I decided to choose an original and specifically local domain for my gem: the events occurring at my local library. I chose the library as a subject because I'm a frequent patron. I often come here for a quiet place to code and grew up borrowing books from this library, carrying home large stacks at a time.
 
	The process of building this gem as a fledgling developer was daunting, but made easier with Avi's video lecture and the use of Bundler. 
 Among other things, Bundler provides the basic file and folder structure for the gem. This is helpful because otherwise the process of learning file and folder structure would add another couple hours to this project. 

	Avi's video lecture on building a CLI gem was invaluable specifically for his tips on what exactly he's thinking about (and thus what I should be thinking about) as he builds the gem. Avi starts with laying out basics of the CLI interface and stubbing the data to begin with. This was a great piece of advice because otherwise, I'd get lost in the nitty-gritty details of the program and fixated on small but relatively unimportant parts. When you start coding an application, like writing, it's more important to get your ideas on the page or screen first without judgement rather than focusing on getting it right. What's the "right" or "wrong" code won't be apparent until we make more decisions and the application starts to take shape. Avi describes his thinking as he codes as, "I'm writing the methods I wish I had". This point is great because as I did so, it helped create the "outlines of the drawing" or the big picture structure of how the classes will interact with each other. Afterwards, I replaced the stubs with actual data scraped from the library's website.

	This project was an incredible learning experience for me mainly due to the problems I encountered. These problems seemed to be related to specific behaviors of creating a gem that I'm not sure could be traditionally "taught" without simply experience them myself through trial by fire. For example, nearing my the release of the gem, I noticed that it would run locally, but not after I installed the built version from Rubygems. This was due to a Nokogiri dependency that needed to be added to the gemspec file. 
	
	Despite it's completion, this gem can definitely be improved in terms of load speed. At the time of this posting, the gem retrieves the external data with a series of each loops. One each loops scrapes each event page's path. Another each loop creates the new URLs with the paths. Another each loop uses these URLs to create the events' HTML docs with Nokogiri. And the final each loop creates an array of hashes containing each event's name, description, date/time, location, and intended audience, scraped from each HTML doc. Obviously the HTML docs are quite large and to create and scrape from 20 of them certainly doesn't help the O(n) of this data structure. I wonder if there's a better way to implement this? 





