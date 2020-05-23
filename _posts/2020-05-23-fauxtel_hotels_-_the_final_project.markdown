---
layout: post
title:      "Fauxtel Hotels - The Final Project"
date:       2020-05-23 18:04:47 -0400
permalink:  fauxtel_hotels_-_the_final_project
---


Well, I said I wanted to be finished with my bootcamp, when I first started, in 4 months or so.  Since then, I’ve had a million soul draining restaurant shifts, two moves to a new apartment, a handful of car troubles, financial worry over my restaurant based income, a viral pandemic and economic lockdown coupled with more anxious thoughts on financial stability.  Four months came and went fast, then I said, well I’ll have it done in July, oh well in September then, ok maybe by the end of the year, so 18 months later, and right up until my administrative deadline for the program, I have finished my last project.

The Project:
I had a few different ideas for a final, but ended up deciding on something I thought was interesting, a hotel brand’s single page application.  Hotel brand websites often don’t do justice to the ease and convenience of booking everything all in one place, such as on websites like kayak.com.

I had a million ideas for how I could address pain points of a hotel brands website with a single page web application and the use of React.

I ended up with a project, for the respect of time and moving forward, that is a good start to a hotel brand’s booking app, and could easily be expanded and improved upon, with time, which I plan to do, of course.

It works, meets requirements and probably goes a little beyond.

For this blog I think I’m going to just do a simple listing of things accomplished or features used:

**React-day-picker:** https://www.npmjs.com/package/react-day-picker
What seems most popular is the integration of a calendar view with user selected dates or date ranges is airbnb’s react-dates npm package.  And yeah it’s fine.  I tried 4 different calendar view packages in a separate program to see which one I liked working with best.  Another reason my project took so long, more than likely!  I ended up choosing, what I think part of react-dates uses actually is react-day-picker.  I came across this while browsing hotel websites to see what they used, or to see if I found any calendar integration that I favored, on https://www.loewshotels.com/.  The understanding of how to style this calendar, enable the user to select a date range and, set dates disabled, I happened to find a little more straightforward to use.  Also with easy access and I plan to integrate it eventually, is the ability to add data to each calendar day.  The original Loew’s hotel calendar I stumbled upon had prices and hotel availability embedded into the calendar view itself, fancy!  I think, not sure, due to hotel closures those date previews have been removed for the time being.

**Action Mailer:** https://guides.rubyonrails.org/action_mailer_basics.html
Action Mailer, is a good time. A little tricky to get setup so you can see that you have actually sent out an email, once all the settings are correct, it is easily integrated into other actions in the controllers.  It was a lot of fun after sitting at my computer for hours seeing an email that came from my localhost:3000.  Currently, I have it set up to send an email on registration of a user and on new reservation.  Plan to do more of course and go further to style the emails.  Overall, I thought it was a quick and easy tool to enhance a project; perhaps also a good learning experience when integrating mailer’s from other backend technologies.


**JWT web tokens:**
I actually had wanted to do something like this for my javascript/rails project, but didn’t bother since it wasn’t required.  Researching and deciding on which method of authentication/authorization to use for a single page web app: tokens, sessions, some other 3rd party integration like firebase or OAuth, was strenuous and probably added a few weeks of time to my project.  Found lots of examples, some using redux but were a little disorganized, some using OAuth but a little tricky to understand as a first time integration.  Also found lots of debates and articles and user comments on the downfalls of security in using tokens and the downfalls of using sessions instead.  I ended up latching onto some of the videos in Learn Instruct on how to go about auth-ing and went with JWT tokens stored in localstorage.  These videos were from globetrotter and react based authentication videos from Howard’s recorded lectures just to give at least semi-formal reference and acknowledgement.  I understand the security implications and if I were to do a hotel web app for a company, I would look into the most secure measures, however, the thing I just wanted to do was understand the process of token based authentication and the basic use of jwt.  Later, I plan to incorporate the session version back into my previous javascript/rails project, for the same purpose and reasoning and, as I move on, I’ll look into some 3rd party integration.

Might be missing other integrations here and there is overall, a great deal more I wanted to incorporate, which I probably will in the near future.  In particular, my plans are to generate reservation codes via a gem available for rails and allow a user to make a reservation without logging in.  I have an interest in incorporating a payment system, airline fares or at least trends and perhaps the “local” weather.  Also wanted to dabble more in React native and would like to do a small mobile app as a companion to this web app that perhaps allows mobile check in and, in theory, door unlocking.  So perhaps these are enhancements to accomplish in the near future!

Github: https://github.com/Tsunamins/FauxtelHotels
Youtube demo: https://youtu.be/G4u_KgDfBYI

