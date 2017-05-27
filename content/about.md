---
title: "Some Shiny Apps"
date: "2016-05-05T21:48:51-07:00"
---

<!--### How to include a video

{{< youtube w7Ft2ymGmfc >}}

### Include an image

{{< figure src="/media/lorenz_plot.png" title="Some Trajectories" >}} -->

### Introduction

The purpose of this page is to show some Shiny Apps of the Lorenz model developed with R and the Shiny package as well as other very usefull packages such as [shinydashboard](https://rstudio.github.io/shinydashboard/), [shinyJS](https://github.com/daattali/shinyjs), [plotly](https://plot.ly/r/), [shinycssloaders](https://github.com/andrewsali/shinycssloaders). Some of these apps are still in development, to improve their design and features.

### Solve the Lorenz model for random initial conditions

This first app simply enables the user to solve the Lorenz model for random initial conditions, without changing parameters. In all my apps, it is possible to save, load, reset and close the application via a button bar.
Finally, as explained in the app text, the maximum number of initial conditions was set to **100**, since above this value, plotly can be unstable on several computers. **A lot of novel features are comming soon**.

<iframe src="https://dgranjon.shinyapps.io/lorenz_1_initialcond/" style="width: 800px; height: 700px; border: none; overflow: hidden;"></iframe>

