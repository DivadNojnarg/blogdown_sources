---
title: "Some Shiny Apps"
date: "2017-05-05T21:48:51-07:00"
---

## Introduction

The purpose of this page is to show some Shiny Apps of the Lorenz model developed with R and the Shiny package as well as other very usefull packages such as [shinydashboard](https://rstudio.github.io/shinydashboard/), [shinyJS](https://github.com/daattali/shinyjs), [plotly](https://plot.ly/r/), [shinycssloaders](https://github.com/andrewsali/shinycssloaders). Some of these apps are still in development, to improve their design and features.

## 1. Solve the Lorenz model for random initial conditions

This first app simply enables the user to solve the Lorenz model for random initial conditions, without changing parameters.
As explained in the app text, the maximum number of initial conditions was set to **100**, since above this value, plotly can be unstable on several computers (my server is 8 cores and 32gb RAM). If you notice that the time to integrate the system is too long, you can reduce the maximum time of integration (or the integration step). **Send me a mail if you find any bug!**

<iframe src="http://130.60.24.205/Lorenz_init/" style="width: 700px; height: 700px; border: none; overflow: hidden;"></iframe>

  * Supported Hardware

This app was tested on Windows 10 (Edge: ok, Mozilla firefox: ok, chrome: ok), macOS Sierra laptop (safari: ok), 8" tablet (chrome and android 7.0), iphone 7 and 5" android smartphone. Besides, on mobile devices, it is not possible to move the 3D graph, due to a compatibility problem with webGL. This is related to the use of **Plotly** package, which seems to only work (totally) with computers.

  * Additional remarks

I started to move all my apps on my private shinyserver. For the moment, only "Lorenz initial conditions"" is ready.

## 2. Solve the Lorenz model when parameters change

The second app provides a simple interface where a control panel allows the user to change parameters, initial conditions together with solver options. Equations are integrated and results are available in a table. Additionally, 3D space graph as well as phase plane projections are displayed on demand. 

<iframe src="https://dgranjon.shinyapps.io/lorenz_2_parameters/" style="width: 700px; height: 700px; border: none; overflow: hidden;"></iframe>

 * Remarks
 
 Last Update: 17/02/18 (update the Initial conditions app:
 - definitely improve the design
 - add new ode libraries (odeintr, RxODE)
 - improve performances (parallel, ...)

<!--### How to include a video

{{< youtube w7Ft2ymGmfc >}}

### Include an image

{{< figure src="/media/lorenz_plot.png" title="Some Trajectories" >}} -->

