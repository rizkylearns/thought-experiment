---
layout: post
title:  "It is Not Easy To Be Single"
date:   2016-04-24 10:00:00 +04:00
categories: ideation
tags: idea concept philosophy
---

I am slowly understanding the significance and practicality of The UNIX Philosophy:
> That the power of a system lies more in the relationships among programs rather than in the programs themselves

I am on the side of these philosphers. I now realize that I have been building monolithic programs all this time. It is not necessary wrong to build these monolithic structures, by the way. Businesses around the world are not exact science; they are not perfectly mappable into a programming model. A lot of UNIX tools are designed to map an exact science, so I can see that a program, for example, functions to search for a specific substring in a text file. Searching for a substring in a text file is an exact science. A specific set of algorithms can be modeled to achieve just that. 
Business models, on the other hand, are not so easily mappable to a programming model like the exact science are. Human needs change, trends change, things that we ask a computing machine to do change over time. Many times we are not sure what exactly do we want out of a program so we program to a "close enough" model, while leaving a room for improvements and change. We want so many things and we cannot untangle our needs into discrete programming models on the first run. So we end up making these programs monolithic enough, tangled enough, "close enough to the real world", in order to solve our daily problems and have our programs help us help ourselves.
Be that as it may, I still set up my mind to focus my works toward creating these system power "cells" instead of gigantic-shiny-ultra-mega-awesome-looking programs for the following reasons:
- The programs will be small; I can deliver them faster
- The programs will be small; I can focus on making a single purpose program in hopes that I won't get confused by what it can do
- They are small and single purposed, so they should be easier to maintain
- "Simple and straightforward" are the remarks I want to get out of these programs I will be making. I want to have more joy of developing these other than taking myself forever to figure out what I have done.

I will be having challenges to achieve them because:
- Breaking down a business challenge into a set of single purpose modules is really, really hard. It takes a lot of trial and errors to produce a "close enough" program while adhering to a single purposeful-ness of each module.
- I have a habit of piling up codes since this is how I was brought up. Just how small is "small"? Should it be less than 10,000 lines? Is this really how I should measure my code's "size"? No, I should not. I have to let it evolve from a multi-purpose program into a set of "I cannot take out anymore out of this" program.
- I have to accept that this is more of art than exact science, with hopes that it will turn into an exact science. To quote myself:
> To have a simple solution that resolves a complex problem means that I understand the problem well. 