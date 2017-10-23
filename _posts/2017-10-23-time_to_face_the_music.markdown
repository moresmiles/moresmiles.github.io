---
layout: post
title:      "Time To Face The Music"
date:       2017-10-23 08:27:00 +0000
permalink:  time_to_face_the_music
---


Although I knew it was coming it was easy to put out of mind. However, when I finally reached the CLI Project, I knew the time had come to face the music, and the music...well it was pretty scary. 

# Wait, we have to do what?
In all of my experience with writing code thus far, I was always given some type of structure to begin with. Sometimes that took form in pre-written files with code in them already, and my task being to edit or add more code. Other times  no previous code was given, but I was given test files, which essentially 'expects' what your code should do. Often times, both. These structures were always there, to guide me to the next step in 'passing' that lab, and always ensured that I had a clue as to what my next step should be.

Enter CLI Project:

* No pre-written files to fork
* No tests
* No right or wrong

The thought of having to do all of this from scratch was quite daunting, and for a few moments seemed insurmountable. And yes, imposter syndrome certainly made an appearance as well.

# Avi's Daily Deal
As per the instructions, I watched Avi's video walkthrough of building a CLI gem called Daily Deal. More accurately, I probably watched it three or four times...and that was before I had even thought about writing any code. 

It was no surprise to me that people write programs from scratch. But to take that out of the theoretical and actually watch someone do so succinctly and successfully step-by-step was something totally different. It helped me realize that while it may not be easy, it was certainly doable.

Besides the confidence I gained from watching the walkthrough, what was perhaps even more helpful was learning an approach to building an application.

Here are some of my main takeaways:

* **Begin with an outline.** The outline serves as a game plan and a guide throughout the process.

* **Start with the entry point.** Write your first line of code at the point where the user will interact with it. In Avi's Daily Deal that was `DailyDeal::CLI.new.call` (a model I incorporated into my program as well). Once you have the point of execution, it makes it easier to write that next line and start encapsulating logic.

* **Don't try to write the whole program at once.**  While this may seem obvious, our brains don't always work that way. Starting with your execution point gives you some type of framework to work backwards and in a way gives you a sense of what your next move should be.

* **Write the code you wish you had.** As Avi mentions multiple times, he likes to "write the code he wishes he had." This was a golden rule for me throughout the entire process. Beginning with the execution point, whose class did not exist at the time of writing, and throughout the entire program I employed this rule. For example, when I defined the #menu method of my CLI class:

```
def menu
 +    input = nil
 +    while input != "3"
      puts "To see our list of major currency pairs, their conversion rates and more information enter 1. To read our disclaimer about currency conversion enter 2, to exit enter 3"
      input = gets.strip
 +    if input.to_i
 +      case input
 +      when "1"
 +        list_pairs
 +        choose_pair
 +      when "2"
 +        disclaimer
 +      when "3"
 +        quit
 +      else
 +        puts "I'm sorry that is not a valid entry."
 +      end
 +      end
 +    end
    end 
``` 
At this point in time, #list_pairs, #choose_pair, and #disclaimer did not even exist, but that was the code I "wished I had", so I wrote it. With that structure I had my next move already laid out for me.

# In Conclusion
While I don't think my CLI app is going to "break the internet", it was certainly ground breaking for me personally in many ways. To name a few:

* Building an entire application from scratch.
* Beating imposter syndrome (for now).
* Gaining confidence.
* Learning an actual approach to building an app from a blank file to a fully functional program.

While this project has been an overall great experience, I am excited to move on and Learn!














