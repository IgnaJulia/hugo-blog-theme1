---
title: A Demo on How to Use Esquiesse in R
subtitle: Starting out with ggplot? Don't feel confident about how you want to visualize your data? Esquiesse offers an amazing starting point.
summary: Learn how to use ggplot in rstudio with the use of the Esquiesse package; great for beginners!
authors:
- admin
tags: [beginners, plot]
categories: [demo]
projects: []
date: "2021-12-02"
lastMod: "2021-12-02"
image:
  caption: ""
  focal_point: ""
---

## Why Use Esquiesse

Esquiesse (French for "sketch") is a package that can be installed in R Studio which provides a GUI-based plot-builder. 

To put it more simply, this package allows you to drag and drop the variables in your data into a chart builder and **voila!** A plot is born. 

When starting out with building plots in R, it can be confusing to know how to write your code to get the plot that you want. While there are many different resources online for picking which chart to use and how to alter your code, many of us beginners struggle with the basics. 

This package uses ggplot to build plots for you, and you can use this tool to grab pre-made code or simply to learn. 

## Let's Get Into It

The package was created by github user [dreamRs](https://github.com/dreamRs/esquisse). It is thanks to creators like them that coding in R becomes a lot more simple and accessible. 

The first thing you need to do is to install the package, which you can do by typing out:

```R
packages.install("esquiesse")
```

And then loading the package by typing:

```R
library(esquiesse)
```

What's even better about the Esquiesse package is that you only need one line of code to activate it. To give us an example, let's use one of the built-in datasets in R. You can use any dataset that is an object type of dataframe. I selected the "ChickWeight" dataset. 

#### An Aside: About This Dataset

The ChickWeight data frame has 578 rows and 4 columns of experimental data on the effect of diet on the early growth of chicks. The 4 columns are:
1. Weight: Body weight of the chick in grams
2. Time: Number of days since birth when the measurement was taken until 21 days old. 
3. Chick: Gives an identifying number for each chick that was included in the experiment
4. Diet: Gives an identifying number for each type of diet condition (four different diets)

You can load this dataset by typing:

```R
data("ChickWeight")
```

To make our data set a bit smaller and more manageable for the purposes of this demo, let's reduce our dataset to include rows with measurements taken on the last day (day 21). 

To do this we will use ```dplyr``` ```filter```. You can install and load this package by using the ```install.packages("dplyr")``` and ```library(dplyr)``` command. 

Once you have that done, let's create our reduced dataset.

```R
Chick_reduce <- ChickWeight %>% 
  filter(Time == 21)
```

You should have a dataset with 45 rows now, each row corresponding to a chick on day 21 after it hatched. 

#### Creating the Plot

So, now that we have our data loaded into our workspace we can use the following command to enter the GUI:

```R
esquisse::esquisser(Chick_reduce)
```

This should give us 

{{< figure src="es_blank.jpg" caption="This should give you a pop-up window that looks like this." >}}

Now we've got a window with all of our column variables at the top and our axes and plot features underneath that. Suppose we want to visualize the changes in weight based on diet. 
- Click and drag 'weight' onto the y axis
- Click and drag 'Diet' onto the x axis

{{< figure src="es_basic.jpg" caption="This should give you a boxplot like the one pictured here." >}}

You could call it a day here, but there's many more features that you could play with to enhance your plot. You may have noticed that before you inputted your variables the plot type (top left icon) was set to automatic. You can pick which plot you'd like to use. We could do a bar graph instead, for example. 

{{< figure src="es_bar.jpg" caption="This should give you a bar plot like the one pictured here." >}}

**Play around by dragging and dropping different variables into different categories.**
- Faceting by chick will give you a separate plot for each chick
- Filling by chick will give you a multicoloured plot, a bit difficult to understand in one visualization due to the number of chicks
- If you attempt to input a variable in a field where the data isn't ideally suited, esquiesse will give you a warning that is easy to understand. 
- Undoing changes is as simple as dragging the variable out of the field. 

#### Labels

Working with our boxplot, click on the "Labels and Title" tab at the bottom of the esquiesse window. Let's input a descriptive title into the title field such as "How 21 Days of Different Diets Affects Chick Weights". If you click on the + button next to the field where you entered the title, you can edit the font and positioning of your title. Let's make our font slightly bigger, size 14.

We can add a caption at the bottom of our plot as well just by filling the "caption" field with: "Protein amounts were varied to create four different diets. Weight after 21 days is pictured above in 48 different chicks". We can also edit the font and centering of the caption. Let's make the caption slightly bigger, size 10. 

To make our y axis a little more informative, we can add a unit by entering "Weight (g)" into the "Y axis" field. Click on the plus button next to the x and y label fields and make the font a little bigger. I chose size 12 font. 

{{< figure src="es_label.jpg" caption="Our plot with informative labels." >}}

#### Aesthetics

Right now our plot looks pretty bland, so it would be great to spruce it up. 

**Colours**

Let's make each box plot a different colour. To do this, drag the diet variable into the "fill" field. Now, each box in our box plot is a different colour. You can edit which colours you want by going into the "appearance" tab and switching the palette (under the palette drop down menu) or you can check "select individual colours" and choose each colour for each box from a colour spectrum. 

- You can also enter a hex code instead of choosing from the colour spectrum, which is useful for if you want to apply colours that are part of some organization's branding, for example. Let's stick with the ggplot theme colours. 

**Legend** 

A legend shows up on the side of our plot which is pretty redundant considering we already have the different diets labelled clearly along the x axis. To remove the legend, click on the "appearance" tab at the bottom of the window. Under "Legend Position" press the "x" button to remove the legend. 

**Grid Lines**

Many times, grind lines can make your plot look cluttered. We can still get an idea of the differences between diet groups without the grid lines so let's remove them. 

Under "appearance" select the "classic" theme. Your grid lines are no more. 

{{< figure src="es_aesthetics.jpg" caption="Our colourful and clean boxplot" >}}











