--- 
title: "R Crash Course"
author: "Robert I. Colautti"
date: "2021-12-02"
documentclass: book
description: "A Crash Course in R for Biologists"
editor_options: 
  chunk_output_type: console
---

# Preface {-}

This book is targeted at the senior undergraduate or junior graduate student with limited training in math, statistics, and computer science. We focus on biological examples in ecology, evolution, genetics, and pathology. I focus on these topics for two reasons. The first is to show how to apply programming tools and techniques to curate, analyze, and visualize biological data. The second is that these are the areas in which I have researched and published papers, and the examples often use the same data. This is not just a matter of convenience but as a demonstration that the tools presented are the very same tools used for rigorous, peer-reviewed research.

This book contains everything I wish I knew when I started programming in R, as a PhD student. In truth, many of the functions and packages provided herein were not available when I was a graduate student in Botany. I continue to add new tricks and techniques that I find useful.

That said, you can't learn to code just by reading this text and the examples. You need to take control of your education. Consider that R is a programming *language*...

> Question: How did you become fluent in a second language? 
  
  * Study, read, listen
  * Try something new, fail, correct errors, repeat


> How do you become fluent in a programming language?
  
  * Study, read, TYPE!
  * Try something new, fail, correct errors, repeat

TYPE! Don't just sit there, follow along and type out the same commands into your R console

Here are a few more tips for successful
  1. Get organized and PLAN. You won't stick to the plan but at least you'll have a blueprint
  2. Set aside large blocks of time (2+ hours), *ideally* without interruption
  3. Get some good headphones with white noise or instrumental sounds (no lyrics), depending on mood:
  
    a. Baroque/Classical
    b. Smooth Jazz  
    c. Electronic (ambient, house, lofi)
    e. https://coffitivity.com/
    
  4. If you get stuck, Google: "How do I ______ in R"
  5. If you can't figure out what an error means, paste it into Google.
  6. Read other people's code carefully to see how they tackle problems. Only rarely is there one single 'right' way to code something. 
  7. When you are starting out, the 'right' way to code is whatever it takes to get the code to do what you want.
  8. **EMBRACE FAILURE!** -- even after ~2 decades of programming experience, most of my algorithms do not work on the first try, and most of my time is spent dealing with errors (typos, mis-specified objects, etc.)!

Learn By Doing!

As you work through these self-tutorials, don't just read them. type them out in your R (Studio) console and see what the output looks like. The simple act of typing it out will help you learn to code! If you get an error, even better! Read the error carefully, then compare what you typed to what is in the tutorial. Once you find what is different, you will learn what that error means.

About 70-90% of coding is dealing with errors, and the same is true for learning to code.

# Setup

Before you begin, you should install the latest version of R:
https://cran.r-project.org/

And then install the latest version of RStudio:
https://rstudio.com/products/rstudio/download/#download

Finally, make sure you are connected to the internet, then open RStudio and locate the 'console' tab. Then install some additional packages

> WARNING! This may take a long time to run

Make sure you are able to leave your computer to update for up to an hour before doing this. To install the packages, type this into your console:


```r
install.packages("tidyverse")
```



