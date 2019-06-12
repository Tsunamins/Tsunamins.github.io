---
layout: post
title:      "Sinatra Project - Miami Rooftop Pools"
date:       2019-06-12 14:59:25 +0000
permalink:  sinatra_project_-_miami_rooftop_pools
---


**“Something to keep track of,”** the project mode page said on learn.co, well I had a million thoughts on things I could use to keep track of, some too simple some too complex.  I thought, not that it is a realistic web app for a variety of reasons, but maybe a simple web based app to keep track of details of various rooftop pools, primarily at hotels, that are easy to visit, have special events where you can visit easily, that hold special events often, or if you had to book a room for a night or two to relax at the pool’s site. 

### File Setup - first things, first
The file setup wasn’t so bad watching the Authentication video and referring back to previous lab examples.  I ran into a little trouble with an error message coming from config.ru, a rake db:migrate error message, did a little searching to see what the problem could be.  I found that I had to change my active record statement from this: 
> 	if ActiveRecord::Migrator.needs_migration?
> 		raise 'Migrations are pending.
> 		Run rake 'db:migrate' to resolve the issue.'
> 	End
	
To this: 

> 	if ActiveRecord::Base.connection.migration_context.needs_migration?
> 		raise 'Migrations are pending. Run 'rake db:migrate' to resolve the issue.'
> 		End



At the moment I don’t remember the error code, but if I can reproduce it or find it, I will add it here.

Excellent, everything is setup and a little test to see that everything is in place:
> 	get '/' do
>    		"Hello World, Can this App Start?"
>  	End 

No. It can't start.

### Run shotgun…...and…...Shotgun Error, or shotgun related error.
After researching this further, I could not run shotgun on windows, or windows as is, I needed Linux or a Linux emulated system in place.
I had a ton of trouble with the learn in browser IDE, I thought no way I’m only working through the learn browser, I will go crazy and abandon my studies.  I almost set it up a week or so prior but held off, so, I decided to take out some extra time during my project and dual boot windows by installing linux and following a couple of setup instructions from learn.co help pages and setup guides (that is another blog post in itself).  It was something I would have to do later, so, all the better to get it set up in advance, and, I like it better!

After getting the whole thing setup, including a few extra steps to get my wifi adapter working in Ubuntu/linux, I cloned my files, ran shotgun successfully and had a look at my ‘/’ route in the browser:
> 	Hello World, Can this App Start?

It could start!! Celebration.

### Continuing the sinatra app setup, what was next?
All the labs had some paralled example to work with, but I knew I may like some of my flow of controllers to work a little differently, but where to get started.  Generally, I worked back and forth between pages and views, how will the first page look, make the page, make that route; how will the sign up page work, make the page, make the route.

This is where I developed a clearer understanding as well between when I was specifying a route and when I was specifing an erb page within other directories.  The previous labs always had similar names, so I was generalizing a lot and putting them into kind of the same category, even though I knew something was different.

The sinatra project was really giving a good opportunity to see exactly what was happening in routes and page rendering and why it was happening, something I was not getting in the labs quite as much.

### Styling - an external css sheet and what to do with it:
Early on while making the pages I thought it best to get started on some styling.  I added some initial style modifications and linked the file in the layout.erb page and...nothing happened.  I looked up a few references to adding css styling in sinatra, found out stylesheets, images, etc. have to be in a directory called `public`. After setting that up, still nothing, until I found out a while later I had the public folder in the wrong place, hanging out in the app folder, it just needed to be a folder at the start of the root of the project.  Css, finally worked after making this change, I then spent about 8 hours seeing how I could make my app look less 1996, semi-successfully.

### Weird sinatra shotgun/terminal errors:
I was working from VSCode via recommendation from one of learn.co’s local environment setup guides and I got the following error after working a while, or something similar.

> start_tcp_server': no acceptor (port is in use or requires root privileges) (RuntimeError)

Cool, what does that mean? After some searching I found a few references to the fact that too many shotgun processes are working at once or a port is already in use, something to that effect, and the new process is interfering with an old running process.  More often than not it has to do with not closing out shotgun properly with `Ctrl+C`.
A variety of solutions are presented to find the processes that are causing the problem within the terminal:

> $ ps
> 
> $ ps ax | grep rails
> 
> $ ps ax | grep thin 
> 
> $ ps ax | grep ruby


All of these return some type of numbers, the solution next to enter in the terminal:
> $ kill -9  #one of the numbers here

I ran into the problem a couple of times while working and found that the most relavant to my current setup was `ps ax | grep ruby`.  Each time I got a list with 2 or 3 entries, different numbers some with question marks, those with question markes were the ones to use to solve the problem.

Overall the project was an excellent way to solidify my understanding of sinatra, routes, CRUD, etc and help me to move on successfully to the next section, Rails.  It may have taken some extra time, but to start working with linux and a local environment a little ahead of schedule I can already see will be a big help when I am ready to start working locally on the rails section, no more IDE crashes, among other things.  I already also have become familiarized with some common errors workign in my own environment, so I think the delay on the project to set up the environment will end up speeding me up in the end.

















