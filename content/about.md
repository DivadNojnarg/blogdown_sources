---
title: "Some Shiny Apps"
date: "2016-05-05T21:48:51-07:00"
---

### Introduction

The purpose of this page is to show some Shiny Apps of the Lorenz model developed with R and the Shiny package as well as other very usefull packages such as [shinydashboard](https://rstudio.github.io/shinydashboard/), [shinyJS](https://github.com/daattali/shinyjs), [plotly](https://plot.ly/r/), [shinycssloaders](https://github.com/andrewsali/shinycssloaders). Some of these apps are still in development, to improve their design and features.

### Solve the Lorenz model for random initial conditions

This first app simply enables the user to solve the Lorenz model for random initial conditions, without changing parameters. In all my apps, it is possible to save, load, reset and close the application via a button bar.
Finally, as explained in the app text, the maximum number of initial conditions was set to **100**, since above this value, plotly can be unstable on several computers. If you notice that the time to integrate the system is too long, you can reduce the maximum time of integration (or the integration step). **Send me a mail if you find any bug!**

<iframe src="https://dgranjon.shinyapps.io/lorenz_1_initialcond/" style="width: 700px; height: 700px; border: none; overflow: hidden;"></iframe>

### Supported Hardware

This app was tested on Windows 10 (Edge: ok, Mozilla firefox: ok, chrome: ok), macOS Sierra laptop (safari: ok), 8" tablet (chrome and android 7.0), iphone 7 and 5" android smartphone. Besides, on mobile devices, it is not possible to move the 3D graph, due to a compatibility problem with webGL. This is related to the use of **Plotly** package, which seems to only work (totally) with computers.

### Additional remarks

For the moment, my apps are hosted on my shinyapps.io account. However, when my shiny server is ready, I will move everything because I am not very satisfied by the shinyapps.io server performances.

<!--### How to include a video

{{< youtube w7Ft2ymGmfc >}}

### Include an image

{{< figure src="/media/lorenz_plot.png" title="Some Trajectories" >}} -->

<!--<iframe src="https://dgranjon.shinyapps.io/lorenz_1_initialcond/" style="width: 700px; height: 500px; border: none; overflow: hidden;"></iframe> -->

