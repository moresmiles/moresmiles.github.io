---
layout: post
title:      "Sinatra Stuff "
date:       2018-01-15 09:26:13 +0000
permalink:  sinatra_stuff
---

 
Unlike the CLI app project, I was less nervous to approach this one. Perhaps it was that I had already made a whole project "from scratch" or maybe having worked hard on the Sinatra NYC and Fwitter labs, I felt more confident. 
 
 
## What to do, what to do... 
 
It took me about two days to decide what type of app to make. My parents had recently moved from my childhood home, and were gracious enough to have all of my belongings that were still there to be put into storage. The problem was I wasn't available when they moved and all of my stuff ended up being packed by movers and eventually put into a storage unit.  
 
The problem I know faced was if i wanted to locate one of my belongings, say my ole' Mets hat, I would have to look through all of the boxes just to find that one item.  
 
I have been coding enough to know there must be better way to do this. Enter Stuff App. 
 
The idea is simple, as you (or even the movers) are packing your items into boxes you simultaneously are entering those items into boxes on the app. Boxes have a Box ID, and a name and  `belong to` a move. While this method does require a bit of manual data entry ( as oppose to some type of scanning system), when all is said and done you have a complete list of where every single one of your items is and where its going, and the best part is you have full access **REMOTELY** to all of your stuff.  
 
Mets hat, here I come (**Box 9** - Hats, Storage Unit) 
 
## Ok, but How? 
 
Firstly shoutout to Flatiron Alum Brian Emory and his awesome corneal gem https://github.com/thebrianemory/corneal .  
 
In his words:  
*A Ruby gem that is a Sinatra app generator with Rails-like simplicity. I built this to help fellow Flatiron School students with their Sinatra assessments.* 
 
Corneal gives you a basic MVC skeleton and a loaded Gemfile with all the gems you have been using in the Sinatra section. Kudus Brian. 
 
I started this project the old fashioned way with a pen and paper. Mapping out all the models and relationships on paper helped a lot, although it still was a bit of challenge. I didn't want to migrate until I was sure everything was set up right.  
 
At first the relationships were like a chain: 
* Owners have Moves 
* Moves have Boxes 
* Boxes have Items 
 
But truthfully, Owners have moves, boxes, and items, and Moves have boxes and items, so  `move_id`, `box_id`, and `item_id` attributes were given to owners, and  `box_id` and `item_id` attributes were given to moves, and Boxes had an `item_id`.  
 
Once all the relational mapping was done, the rest of the program writing went rather smoothly. 
 
![Early mapping](https://imgur.com/2iglfu2) 
 
Having just finished the Sinatra section and specifically the Sinatra NYC and Fwitter labs, I felt rather comfortable with RESTful routes and CRUD actions. While having four controllers in addition to the `ApplicationController`, created a lot of routes and views to keep track of, for the most part a lot of them had very similar functionality, and didn't prove too big of a challenge. 
 
 
## Helpers 
 
The name says it all. Helpers, they help. Let them help. 
 
`logged_in?` basically proceeded any action, as we wouldn't want a user who's not logged in access to parts of the program that require being logged in, and for sure not access to another user's data. 
 
`current_user` and `current_move` were also big helpers. 
 
`redirect` to index, login, and owner pages were nice shortcuts as well. 
 
## CSS how does that work again? 
 
After completing the functionality of the app, the result was lacking any type of style.   
 
Although we had learned CSS way earlier in the curriculum, I don't think it was utilized very much since then, if at all. So essentially, I had forgotten how to use it. I decided that I would "re-learn" CSS in a day or so, and be able to give my app some flair. However instead of actually trying to learn it, what I was really looking for was some quick fixes to make the app look presentable.  
 
After a couple of days of this I was getting a bit frustrated. I dabbled in bootstrap, but it seemed way to large for such a small project as this. I ended up added a much lighter styling in skeleton http://getskeleton.com/ .  
 
In their words: 
*A dead simple, responsive boilerplate. Light as a feather at ~400 lines & built with mobile in mind.* 
 
I made a bit of headway in the CSS, but after spending more time on the styling than the actual program, I decided to stop. For some odd reason the CSS would work for some pages and not for others, unclear if it's skeleton, shotgun, or just my errors. 
 
What is really missing from this app is a navigation bar. I tried a few different ways but couldn't get it the way I wanted, so for now each page just has buttons to direct you.  
 
I know I need to take time to properly learn how to style as it is an integral part of being a "full stack" developer, but for now I was getting a bit too burnt out. Perhaps in the upcoming sections Rails and then JS, styling will be a bigger part of the lessons. 
 
That being said I did manage to style a few pages that I am proud of. 
 
![Home Page](https://imgur.com/ffqHv1P) 
 
![Log In Page](https://imgur.com/AeVOj5C) 
 
## Conclusion 
 
As imperfect as this app is, it is still the first web app that I made from scratch. I also had a really fun time coding it, there were many moments of "flow", after which I looked up to see that **hours** had passed. This made me quite happy, knowing that I could get into that flow state and work for hours at a time. 
 
All in all I think **Stuff** is actually a really useful idea, and I would love to come back to it once I have more knowledge (and for sure styling).  
 
I am going to leave it for now, and I am excited to discover the "magic" of Rails.
 
