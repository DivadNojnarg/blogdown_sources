---
title: "Some Shiny Apps"
date: "2017-05-05T21:48:51-07:00"
---

## The Lotka Volterra Model

### Introduction

The study of prey-predators interactions is a very exciting field, where a wide variety of models is available. The most famous is obviously that of Lotka-Volterra created in parallel by Vito Volterra and James Lotka (V. Volterra, Nature, 118: 1926). This model contains shortcomings such as the infinite growth of prey, which is not realistic (Malthus growth rate). It is indeed more accurate to use a Verhulst growth function, which takes into consideration competition between preys. Since then, a lot of model were developed, with different functional responses, each characterising prey-predators interactions: Holling 1, Holling 2 (Rosensweig-MacArthur), Holling-Tanner, Beddington... The following app aims at providing a comprehensive tool to investigate the dynamics of such systems. Basic phase plane analysis is included: equilibrium points, stability. For the moment, Lyapunov functions or more sophisticated tools are not present. Thus a lot of mathematical features will be implemented as soon as possible. Besides, local and global sensitivity analysis can be tested with the first "basic Lotka Volterra model". Finally, only positive equilibriums are considered, for biological consistency. <br/>

Click [here](http://130.60.24.205/Lotka_bdd/) to acces my App.

<a href="http://130.60.24.205/Lotka_bdd/"><img src="images/demo_lotka_1.png" width="auto" height="auto" alt="blank"></a>
<a href="http://130.60.24.205/Lotka_bdd/"><img src="images/demo_lotka_2.png" width="auto" height="auto" alt="blank"></a>

Last update: 18/02/18 -> Major update of Lotka Volterra App (design, ...)

## A model reader
### Introduction
  
All my previous shiny apps are dedicated to special models and features tighly depend on model properties. Thus, came the idea of making a shiny app which will be able to read any 2D model, drawing the phase plane and making a stability analysis. The following app is a first step toward a much more sofisticated one, and is of course still in development. <br/>
  
Click [here](http://130.60.24.205/Model_Reader/) to acces my App.

<a href="http://130.60.24.205/Model_Reader/"><img src="images/demo_model_reader_2.png" width="auto" height="auto" alt="blank"></a>
  
  
Last update: 18/02/18 -> Major update of design and improvements (see App)

## The Lorenz Model

### Introduction

The purpose of this page is to show some Shiny Apps of the Lorenz model developed with R and the Shiny package as well as other very usefull packages such as [shinydashboard](https://rstudio.github.io/shinydashboard/),
[shinyJS](https://github.com/daattali/shinyjs), [plotly](https://plot.ly/r/),
[shinyWidgets](https://github.com/dreamRs/shinyWidgets),
[shinycssloaders](https://github.com/andrewsali/shinycssloaders). Some of these apps are still in development, to improve their design and features.

### 1. Solve the Lorenz model for random initial conditions

This first app simply enables the user to solve the Lorenz model for random initial conditions, without changing parameters.
As explained in the app text, the maximum number of initial conditions was set to **100**, since above this value, plotly can be unstable on several computers (my server is 8 cores and 32gb RAM). If you notice that the time to integrate the system is too long, you can reduce the maximum time of integration (or the integration step). **Send me a mail if you find any bug!**<br/>

Click [here](http://130.60.24.205/Lorenz_init/) to acces my App.

<a href="http://130.60.24.205/Lorenz_init/"><img src="images/demo_lorenz_initcond.png" width="auto" height="auto" alt="blank"></a>
<a href="http://130.60.24.205/Lorenz_init/"><img src="images/demo_lorenz_initcond_2.png" width="auto" height="auto" alt="blank"></a>

## 2. Solve the Lorenz model when parameters change

The second app provides a simple interface where a control panel allows the user to change parameters, initial conditions together with solver options. Equations are integrated and results are available in a table. Additionally, 3D space graph as well as phase plane projections are displayed on demand.<br/>

Click [here](http://130.60.24.205/Lorenz_parameters/) to acces my App.

<a href="http://130.60.24.205/Lorenz_parameters/"><img src="images/demo_lorenz_parameters_1.png" width="auto" height="auto" alt="blank"></a>
<a href="http://130.60.24.205/Lorenz_parameters/"><img src="images/demo_lorenz_parameters_2.png" width="auto" height="auto" alt="blank"></a>

### 3. Other details

#### Supported Hardware

This app was tested on Windows 10 (Edge: ok, Mozilla firefox: ok, chrome: ok), macOS Sierra laptop (safari: ok), 8" tablet (chrome and android 7.0), iphone 7 and 5" android smartphone. Besides, on mobile devices, it is not possible to move the 3D graph, due to a compatibility problem with webGL. This is related to the use of **Plotly** package, which seems to only work (totally) with computers.


#### Remarks
 
 Last Update: 04/01/19
 
 + update article

<!--### How to include a video

{{< youtube w7Ft2ymGmfc >}}

### Include an image

{{< figure src="/media/lorenz_plot.png" title="Some Trajectories" >}} -->

