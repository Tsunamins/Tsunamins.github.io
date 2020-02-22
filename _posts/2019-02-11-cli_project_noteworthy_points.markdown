---
layout: post
title:      "CLI Project, Noteworthy Points"
date:       2019-02-11 20:49:53 -0500
permalink:  cli_project_noteworthy_points
---


The idea of my CLI project is things to do in Miami today (or another date specified), scraping from Perez Art Museum and the Frost Science Museum’s Laser Friday’s page.  Of course there are many more things to do in Miami on any given day, but the trick of this project was to scrape but also just make a nice clean code.
Project Setup:
File setup was a little tricky when first creating this gem set of files, it is handy however that initial file structure and the necessary inclusions, requirements and calls to certain files are incorporated automatically.  However the file setup is simplified, there are a few details to iron out in advance and upon project setup: 

## Project naming: 

A different file structure is created with something such as miami_venues_today and miami_venues. 
•	`miami_venues_today` will create several layers of files, one nested under the other, the file structure ends up looking a little more complicated `lib/miami/venues/today`, but also affects the require parameters and adds unnecessary files to a simple project.  Other projects could perhaps use such a naming and file system, however, so it is still useful to know. 
•	miami_venues will create a less file levels and nests making the require parameters much more simple and manageable, `lib/miami_venues`. 
These initial naming and file conventions are however manageable in either case, files can be easily deleted, added, renamed and the overall file structure changed, one just needs to makes sure all file inclusions match throughout the project’s files. 
The .gemspec file:
miami_venues.gemspec, for example has a great deal of specifiers for publishing which need to be filled out, mostly anything that has a “`TODO`” listed needs to be filled out with the appropriate value, or this publishing section could use to be commented out. 
Most importantly in the .gemspec file is the bottom where various dependencies are added.  Some are added initially, but for this particular project it is also necessary to add the lines: 

```
spec.add_development_dependency "pry
spec.add_development_dependency "nokogiri"
```

	 
These in place in the .gemspec file make sure the project can scrape and one can use pry while working on the code in the files. 

## Config or environment files:

The first file within lib added is `miami_venues.rb`, it is the file to make sure use of all the other files is incorporated, it is the environment and configuration type of file.  I was used to the environment.rb file in a config folder from labs so set it up like one of the example projects where a config folder is added, environment.rb includes the necessary files and the lib/miami_venues.rb includes the environment file.  Probably a little unnecessary but also interesting to see exactly how that file structure is initially created while creating a new gem, and good to know how all those files work together.

## Learned some new things – dates and ranges

At some point whether in place yet or not, the CLI would have to ask for some user input and match today’s date, match some other specified date to what the user wants to find.  The date specified would have to easily match to a list of event dates and pull those details up for the user.  Each of the two websites I was interested in scraping had not only different date format’s within the text of an element, but had date ranges to work with as well.  So, I had to research the ruby date methods.  In order to use various the predefined date class and its methods, require ‘date’ had to be included at the top of the file along with pry, nokogiri and open-uri.  Three date methods were extremely useful, `DateTime.Parse()`, `.strftime()` and `Time.new`:

`DateTime.parse(‘date here in almost any format’)`, changes a date such as January, 1 2018 and makes the value a standardized version of time: 

![](https://i.imgur.com/4Tki4Ww.jpg/)
	
Once this date was standardized a specific formatted output can be returned with `.strftime(“'%a %d %b %Y'”)`, these representing name value of the day, numerical value of the day, month and year, respectively : 

![](https://i.imgur.com/40DVLJ7.jpg)
	
These methods were included in a helper method to pass in any date data gathered from the website and process them back out with the format above, in order to match the user date specified later.
Within the CLI if the user wants to check events for the current date and time, Time.new is called set to a variable.  In order to display this time, puts needs to be used in conjunction with this time, for my program I utilized `.strftime()` to standardize to set up for matching with events:

![](https://i.imgur.com/1SFcCVD.jpg)
![](https://i.imgur.com/5TW5UGL.jpg)

### Self.ishness: 

So, at first I wasn’t thinking ahead and forgot all about the use of self and why it is used and how it is used and what the difference is, etc.  I generally understood what it was doing in labs but certainly couldn’t put it into words yet.  So, once I tried to combine my classes together and get everything functioning in the CLI, I was getting errors and why, oh, that’s why without defining my scraper methods as `self.scrape_something`, and just `scrape_something`: trying `find_scraped_items.scrape_something` was returning an error.  After changing to `self.`, I could successfully interact my CLI, my Events class and my Scraper class together, setting variables equal to interacting objects.  Also, learned that unless called in a very specific way, you can’t call an instance method within a class method, suddenly my date changing method wasn’t working, however just calling this `self.change_date_format`, or whatever it was called, and making that modification within the call to itself fixed this quickly.

### I discovered an em dash:

Or was it an en dash?  I’m not sure and I kind of find it upsetting there is some difference between a hyphen “-“ an en dash “–”, and apparently one other similar larger than hyphen larger than en dash sized – em dash, Microsoft word makes an em or en automatically in certain circumstances, so that’s great.  However, if you are attempting to make something such as `.split(“-“)` find a split, in my example a date range, work when there is actually an em or en dash in the middle, ruby won’t catch it.  I tried to look up character encoding and how I may use that, none of that was clear so I decided to see if I could use regex somehow.  By inserting `/\W[^ ,A-Z,1-9]/ ` into `.split(/\W[^ ,A-Z,1-9]/)`, I was able to exclude every character necessary except anything resembling a hyphen or dash and was finally able to proceed hours later!!!!  Regex is really a good time!

> October 4, 2018 - February 17, 2019 (hyphen)         July 21, 2017 – Sept. 1, 2019 (em or en dash, roll eyes)                     

After all the various tricks and pondering of life my CLI project took I am glad to have experienced it.  There were several other stuck points possibly worth highlighting but these seemed the most useful and interesting.








	

