---
layout: post
title:      "Prototype I: A Cat's Dream - JS/Rails Project"
date:       2020-01-04 19:55:22 +0000
permalink:  prototype_i_a_cats_dream_-_js_rails_project
---


JavaScript was a little weird, why do I have to have these functions that need other functions or to call other functions from other functions to get something to work almost making it look more like an alternative loop rather than a function?  Eventually it all became clearer, so that’s fine.

Time for the project, a single page web application.  I browsed the project examples and wasn’t sure what I would want to do, but my first thought was some type of game that moves a character around on the screen via keyboard controls and does something fetch-worthy.
  
I started looking into javascript based games, hoping to “keep it simple” by just writing something from vanilla javascript.  I started down a path of viewing and working with a variety of youTube tutorials, basically making mini game engines:

> JavaScript Teacher `https://www.youtube.com/channel/UCzQvgRgjjxhzEORvefubDPw`
> 	
> Technologies4me `https://www.youtube.com/channel/UCHpHBzk4fz3oeQ31hmCreGg`
> 
> PothOnProgramming `https://www.youtube.com/channel/UCdS3ojA8RL8t1r18Gj1cl6w`



During these I discovered the process of converting a png image into a tile map based on parsing array values into pixel based coordinates of the png image.  I discovered the use of Tiled to make a tile map with a little more ease rather than typing in the array values needed, manually.  Along with this I found a lot of details about animating images based on sprite sheets with different character positions. I found an excellent tutorial on the use of the program Tiled as well

> GamesFromScratch `https://www.youtube.com/channel/UCr-5TdGkKszdbboXXsFZJTQ`
	
However all tutorials were extremely interesting and amazingly detailed, it certainly wasn't "keeping things simple."


It was wonderful to learn how to work with computer language to get positions of the mouse, keys, input, tile types and set up collision and I wanted to go through and understand all of them thoroughly, however, I worried over the time it would take me to custom make my own version of these games.  The Tiled tutorials brought my attention to Phaser, a game engine built in javascript, and I was reluctant to try it right away to the point I spent some days brainstorming other ideas for fear of this whole tile game idea was going to take me too long and turn out to be a bad idea.

I eventually went onto Phaser’s website

> `https://phaser.io/`
	
They have a great introduction to their engine on their website including a very simple tutorial on how to make a first game.  I got through the tutorial in a couple hours with a pretty much fully functional mini game.

Phaser already has collisions, overalaps, camera settings, scene settings, gravity attributes, colors, text setting, position getting/setting of every game component and a library of nearly every type of game need, organized and ready to go, and if you need something else you can add a custom class of whatever you feel like.  Additionally, they have a great deal of user contributed tutorials and examples to get more ideas or see how something can work, an amazing documentation site and links to many other resources.  Once I got the hang of Phaser as well, it was extremely easy to figure out how to manipulate my own objects and game elements by console.log(ing) the game element in question and having a look at its lengthy list of attributes, very intuitive from there on.

I discovered other game making resources, free to use game art, for example, from `https://opengameart.org/ `.
However, I couldn’t help but want to make my own art, got a little stuck on what my game art theme and game may even be, so, while working through various phaser tutorials I started attempting my own game art, primarily on pixilart.com `https://www.pixilart.com/draw#`
I eventually decided to use a random image I drew with Krita drawing software, a pink night sky with silhouetted black grass and trees as sort of a game’s title scene or landing page and figured to make game art based on this image, thus I came up with my game art theme.  I made my animation frames for the player, enemy and an item to collect all on pixilart.com.

Many of these ideas and possibilities of integration came from the very first phaser tutorial showing player movement based on keyboard input, collision detection and player animation.  Previously, with vanilla javascript I was ready to settle for a player with no animation that perhaps moved around and collected something on boring map and did nothing else.

Working through further Phaser tutorials with zenva `https://academy.zenva.com/`
I found a great deal of tilemap based games with a great deal of examples of different elements to add to a game and different playing styles.  I had planned to add a few more such as NPC’s and different scene or level transitions, but in the interest of time I left it to one, at least for now.

I’m not sure what I would have come up with had I decided to keep this project simple and do enough just to meet the requirements, but I’m glad I ventured into some processes of game making in collaboration with web development.

