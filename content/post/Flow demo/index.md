---
title: A Demo on How to Use the Flow Package in R
subtitle: Your data tells a story and so does your code. Paint a picture using Flow.

# Summary for listings and search engines
summary: Flow charts can be extremely useful for when you're starting a new project. This demo shows R users how they can easily generate a flow chart to depict the steps their code is taking in execution. 

# Link this post with a project
projects: []

# Date published
date: "2021-12-01"

# Date updated
lastmod: "2021-12-01"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**StockImage**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- admin
- 

tags:
- Visualizing
- 

categories:
- Demo
- 
---

## Why Use Flow?

Beginner coders (and even those who have more experience) often grapple with the difficulties of understanding the logic of functions, expressions or scripts. Flow is here to help. 

If you are trying to do any of the following in **Rstudio**: 

1. Understanding other people's code
2. Understanding your own code
3. Debugging
4. Inspecting unit test results
5. Providing a higher level view when working with collaborators
6. Educating others

then this demo is for you. 


{{< figure src="flow.jpg" caption="Here is an example of a flow chart, something that Flow will produce for you." >}}

## Let's Get To It!

The first step in utilizing flow is to install and load the package. The package was created by Antoine Fabri, so big thank you to him. You can see the documentation for this package at [MoodyMudskipper](https://github.com/moodymudskipper/flow). You can install and import Flow by typing the following into your markdown document or console:

```R
install.packages("flow")
library("flow")
```

## Flow_view VS Flow_run

Flow has two main functions that are of interst to us. We can use ```flow_view``` for when we just want to look at a function's logic on its own. This will give us a chart that shows how a given function will take us from point A, B, C... etc to our final output. ```flow_run``` can be used for when we want to see the path the function took to give us an output when we actually run it with some data. Let's start with looking at the functionality of ```flow_view```. 

To begin, let's make a mock data set. I created a vector of 15 numbers, negative and positive. 

```R
numbers <- c(5, 2, -4, 3, 2, -1, -7, -9, 4, 5, -8, -2, 10, -10, -3)
```

{{< figure src="numbers.jpg" caption="This gives us a nice list of numbers when we print the object numbers in the console." >}}

Now, let's supposed that we'd like to create a function that tells us when our number is positive and when it is negative. 

```R
pos.or.neg <- function(numbers){
  for(number in numbers){
    if(number >= 0){print("This number is positive!")}
    else{print("This number is negative!")}
  }
}
```

Great! Now we have a function. Let's take a look under the hood by using ```flow_view```

### Flow_view

```R
flow_view(pos.or.neg)
```

{{< figure src="flowview.jpg" caption="Coloured circles are exit points, orange for errors, green for returned values, or when the function ends." >}}

By looking at this flow chart we can see that the ```pos.or.neg``` function we created will go through the following steps:
- Function is called
- ```for``` every number in the numbers object (which is our list of objects):
- Logical expression: ```if``` the number is greater than or equal to 0 THEN:
  - Print out "This number is positive!"
  - ```if``` it isn't THEN:
    - Print out "This number is negative!"
- Repeat this step until the function has gone through all numbers in the list. 

The ```flow_view``` function has shown us that our logic seems to make sense. Now to see our function in action, let's use ```flow_run```.

### Flow_run

```R
flow_run(pos.or.neg(numbers))
```

If we use the above code then our flow chart remains *almost the same as for ```flow_view``` (because it is following both pathways), and we get an output that looks like this: 

{{< figure src="flowrunall.jpg" caption="A statement about the valence of each number." >}}

{{< figure src="flowrunchart.jpg" caption="Look closely at the differences between this flow chart and the one generated by flow view." >}}

*```flow_run``` gives us a count of how many times each path was taken. By looking at the numbers next to each block, we can see that 15 code blocks were entered into the green triangle (```if``` statement), and that 7 of those code blocks produced the output "This number is positive" and 8 produced "This number is negative". The loop repeated 14 times. 

So, we can tell our function is doing what it's supposed to be doing. To investigate the path that our function is taking for a specific element in our list of numbers, let's use the following code:

```R
flow_run(pos.or.neg(numbers[[3]]))
```

The third element in our list of numbers is -4, so it should print out "This number is negative!" 

{{< figure src="flowrunneg.jpg" caption="The dotted line shows the path that the function did not take." >}}

Here, we can visually confirm that the function followed the appropriate steps to produce the correct output. The path is highlighted by the solid lines, whereas the dashed/dotted line is a visualization of the path that was ignored following the logical expression. Since we asked the function to give us an output for just the 3rd element in our list of numbers, you can see that the loop wasn't implemented. 

I can now take this visualization and use it to explain to others how my code works. I can also use it to double check that my code is handling the data the way it is supposed to, saving me time and energy down the line had I made an error. You can use flow on much more complicated functions and get even *more* utility out of it too!

## Pre-existing Functions

We've gone through an example with a function that we created. We can also use ```flow_view``` and ```flow_run``` on functions that we did *not* create. What's useful with this is that we probably have a better understanding of what we are writing than the "black box" that is a pre-written function. 

We can take a commonly used function such as  ```add_row``` from the ```dplyr``` package and see the logical break down. 

{{< figure src="add_row.jpg" captio="That's a lot of steps for one function!" >}}

This may look overwhelming, but it contains a lot of useful information, especially if you're unfamiliar with the function. We can see where the function will spit out an error/warning, what kind of object our data needs to be in, and what it will output. We even see which functions it is relying on within it, and so on. This is key to seeing inside the afforementioned "black box" and taking your comprehension to the next level. 

### When It Isn't Useful...

Sometimes, flow isn't as informative. Let's look at an example. 

Take the ```tidyr``` function ```pivot_longer``` and put it into ```flow_view```. You should get the following chart: 

{{< figure src="pivot_longer.jpg" caption="The least useful flow chart ever." >}}

This is because the function that we wanted to explore uses javascript binding, and so its steps are hidden. 


## Extra Utility

### Generating Polished Charts

You may have noticed that flow produces charts that can look a little... unfinished. You can get cleaner, more vibrant-looking flow charts by using the plantuml engine. All you need to do is install the ```plantuml``` package, and alter your code as follows:

```R
flow_view(pos.or.neg, engine = "plantuml")
```

### Exporting

If you want to export the flow charts, either to zoom in and explore the image closer, or to have a copy outside of R, then you can use the following code:

```R
#Export as a temp pdf file
flow_view(pos.or.neg, out = "pdf")
#Export to a folder
#Replace "some folder" with your desired location
flow_view(pos.or.neg, out = "C:/some_folder/posorneg.jpeg")
```
You will need to have ```phantomjs()``` installed which you can do by running the command ```webshot::install_phantomjs()```. 

You can replace pdf and jpeg with different file formats, of course. 

### If You're Keen For More...

You can play around with ```flow_view``` and ```flow_run``` with many different functions. Loops such as ```if```, ```for```, ```while```, and ```repeat``` are all supported. 

Flow has other functionalities aside from ```flow_view``` and ```flow_run``` though they are more advanced. You can debug, and even generate flow charts for entire packages! You can access the manual [here](https://cran.r-project.org/web/packages/flow/index.html) and follow the flow.pdf link. 

## Summary

Hopefully this demo has piqued your interest and will encourage you to test out flow on your code, and will give you the confidence needed to try to understand functions that you would have previously thought were too complicated and too out-of-reach. 