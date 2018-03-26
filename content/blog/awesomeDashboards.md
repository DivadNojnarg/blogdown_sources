+++
  title = "Build awesome dashboards with shiny"
  date = "2018-03-26"
+++

## Introduction

[shinydashboard](https://rstudio.github.io/shinydashboard/) is without any doubt a great package.
However, most of the dashboard I see almost look the same. However, one does not have to forget that shinydashboard
is built upon the famous free [adminLTE2](https://adminlte.io/themes/AdminLTE/index.html) dashboard template (that uses boostrap 3).
Indeed, if you look a the shinydashboard github repository, there is a folder inst/AdminLTE.

## From R to HTML

You probably know that when you write a R shiny code, such as **sliderInput**,
shiny will generate the corresponding HTML code, to be integrate into an html page. 
Let's have a look at what happens:

```{r}
library(shiny)
sliderInput("obs", "Number of observations:",
    min = 0, max = 1000, value = 500
  )
```

If you enter the previous code in the R console, you will see:

```{html}
<div class="form-group shiny-input-container">
  <label class="control-label" for="obs">Number of observations:</label>
  <input class="js-range-slider" id="obs" data-min="0" data-max="1000" data-from="500" data-step="1" data-grid="true" data-grid-num="10" data-grid-snap="false" data-prettify-separator="," data-prettify-enabled="true" data-keyboard="true" data-keyboard-step="0.1" data-data-type="number"/>
</div>
```

Basically, all what you write in R will be translated into HTML:

```{r}
library(shiny)
library(shinydashboard)
shinyApp(
  ui = dashboardPage(
    dashboardHeader(),
    dashboardSidebar(),
    dashboardBody(),
    title = "Dashboard example"
  ),
  server = function(input, output) { }
)
```

The UI code will be translated into:

```{html}
<body class="skin-blue" style="min-height: 611px;">
  <div class="wrapper">
    <header class="main-header">
      <span class="logo"></span>
      <nav class="navbar navbar-static-top" role="navigation">
        <span style="display:none;">
          <i class="fa fa-bars"></i>
        </span>
        <a href="#" class="sidebar-toggle" data-toggle="offcanvas" role="button">
          <span class="sr-only">Toggle navigation</span>
        </a>
        <div class="navbar-custom-menu">
          <ul class="nav navbar-nav"></ul>
        </div>
      </nav>
    </header>
    <aside class="main-sidebar" data-collapsed="false">
      <section class="sidebar"></section>
    </aside>
    <div class="content-wrapper">
      <section class="content"></section>
    </div>
  </div>
</body>
```

In pratice, this is not enough to make beautiful dashboard but its a good start.
With this very simple technic, you will be able to generate any **custom HTML** code you want.
For example, you are creating a shiny app with an [**HTML template**](https://shiny.rstudio.com/articles/templates.html). 
I find easier to create inputs (slider, checkboxes) writing them in R and copy the code to the R console
to get the corresponding HTML, and include this HTML snippet in your full template, than
directly writing it in HTML, especially if you are not an HTML nerd. An example below:

```{r}
library(shinyWidgets)
prettyCheckboxGroup(
  inputId = "checkgroup2",
  label = "Click me!", thick = TRUE,
  choices = c("Click me !", "Me !", "Or me !"),
  animation = "pulse", status = "info"
)
```
This code is only 7 lines long and very clear. The HTML translation below
is widely less clear:

```{html}
<div id="checkgroup2" class="form-group shiny-input-checkboxgroup shiny-input-container">
  <label class="control-label" for="checkgroup2">Click me!</label>
  <div class="shiny-options-group">
    <div style="height:7px;"></div>
    <div class="pretty p-default p-thick p-pulse">
      <input type="checkbox" name="checkgroup2" value="Click me !"/>
      <div class="state p-info">
        <label>
          <span>Click me !</span>
        </label>
      </div>
    </div>
    <div style="height:3px;"></div>
    <div class="pretty p-default p-thick p-pulse">
      <input type="checkbox" name="checkgroup2" value="Me !"/>
      <div class="state p-info">
        <label>
          <span>Me !</span>
        </label>
      </div>
    </div>
    <div style="height:3px;"></div>
    <div class="pretty p-default p-thick p-pulse">
      <input type="checkbox" name="checkgroup2" value="Or me !"/>
      <div class="state p-info">
        <label>
          <span>Or me !</span>
        </label>
      </div>
    </div>
    <div style="height:3px;"></div>
  </div>
</div>
```

But what if you find an interesting HTML object (boostrap 3 compatible) 
that you want to integrate in your shiny app?

## From HTML to R

### Import HTML code into shiny

Let's start with this very preliminary shinydashboard template:

```{r}
library(shiny)
library(shinydashboard)
shinyApp(
  ui = dashboardPage(
    dashboardHeader(),
    dashboardSidebar(),
    dashboardBody(),
    title = "Dashboard example"
  ),
  server = function(input, output) { }
)
```

You noticed a timeline element that you really want to include in your shinydashboard:

<a href="images/timeline_example.png"><img src="images/timeline_example.png" width="auto" height="auto" alt="blank"></a>

First of all, download the adminLTE2 source from [here](https://adminlte.io) (version 2.4.2).
Then, according to the following screenshot, open the profile.html page with your
favorite editor (notepad ++, Rstudio, Xcode, ...). 

<a href="images/adminLTE2_folder.png"><img src="images/adminLTE2_folder.png" width="auto" height="auto" alt="blank"></a>

Go to find, type "The timeline" and copy the following code, which corresponds 
to the timeline object:
```{html}
<div class="tab-pane" id="timeline">
                <!-- The timeline -->
                <ul class="timeline timeline-inverse">
                  <!-- timeline time label -->
                  <li class="time-label">
                        <span class="bg-red">
                          10 Feb. 2014
                        </span>
                  </li>
                  <!-- /.timeline-label -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-envelope bg-blue"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 12:05</span>

                      <h3 class="timeline-header"><a href="#">Support Team</a> sent you an email</h3>

                      <div class="timeline-body">
                        Etsy doostang zoodles disqus groupon greplin oooj voxy zoodles,
                        weebly ning heekya handango imeem plugg dopplr jibjab, movity
                        jajah plickers sifteo edmodo ifttt zimbra. Babblely odeo kaboodle
                        quora plaxo ideeli hulu weebly balihoo...
                      </div>
                      <div class="timeline-footer">
                        <a class="btn btn-primary btn-xs">Read more</a>
                        <a class="btn btn-danger btn-xs">Delete</a>
                      </div>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-user bg-aqua"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 5 mins ago</span>

                      <h3 class="timeline-header no-border"><a href="#">Sarah Young</a> accepted your friend request
                      </h3>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-comments bg-yellow"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 27 mins ago</span>

                      <h3 class="timeline-header"><a href="#">Jay White</a> commented on your post</h3>

                      <div class="timeline-body">
                        Take me to your leader!
                        Switzerland is small and neutral!
                        We are more like Germany, ambitious and misunderstood!
                      </div>
                      <div class="timeline-footer">
                        <a class="btn btn-warning btn-flat btn-xs">View comment</a>
                      </div>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <!-- timeline time label -->
                  <li class="time-label">
                        <span class="bg-green">
                          3 Jan. 2014
                        </span>
                  </li>
                  <!-- /.timeline-label -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-camera bg-purple"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 2 days ago</span>

                      <h3 class="timeline-header"><a href="#">Mina Lee</a> uploaded new photos</h3>

                      <div class="timeline-body">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                      </div>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <li>
                    <i class="fa fa-clock-o bg-gray"></i>
                  </li>
                </ul>
              </div>
```

To include raw HTML code in shiny, simple write:
```{r}
HTML(
  paste0('
  Your HTML code here
  ')
)
```
Let's go back to our shinydashboard and include this HTML code in the body part:

```{r}
library(shiny)
library(shinydashboard)
shinyApp(
  ui = dashboardPage(
    dashboardHeader(),
    dashboardSidebar(),
    dashboardBody(
      HTML(
        paste0('
          <div class="tab-pane" id="timeline">
                <!-- The timeline -->
                <ul class="timeline timeline-inverse">
                  <!-- timeline time label -->
                  <li class="time-label">
                        <span class="bg-red">
                          10 Feb. 2014
                        </span>
                  </li>
                  <!-- /.timeline-label -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-envelope bg-blue"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 12:05</span>

                      <h3 class="timeline-header"><a href="#">Support Team</a> sent you an email</h3>

                      <div class="timeline-body">
                        Etsy doostang zoodles disqus groupon greplin oooj voxy zoodles,
                        weebly ning heekya handango imeem plugg dopplr jibjab, movity
                        jajah plickers sifteo edmodo ifttt zimbra. Babblely odeo kaboodle
                        quora plaxo ideeli hulu weebly balihoo...
                      </div>
                      <div class="timeline-footer">
                        <a class="btn btn-primary btn-xs">Read more</a>
                        <a class="btn btn-danger btn-xs">Delete</a>
                      </div>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-user bg-aqua"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 5 mins ago</span>

                      <h3 class="timeline-header no-border"><a href="#">Sarah Young</a> accepted your friend request
                      </h3>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-comments bg-yellow"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 27 mins ago</span>

                      <h3 class="timeline-header"><a href="#">Jay White</a> commented on your post</h3>

                      <div class="timeline-body">
                        Take me to your leader!
                        Switzerland is small and neutral!
                        We are more like Germany, ambitious and misunderstood!
                      </div>
                      <div class="timeline-footer">
                        <a class="btn btn-warning btn-flat btn-xs">View comment</a>
                      </div>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <!-- timeline time label -->
                  <li class="time-label">
                        <span class="bg-green">
                          3 Jan. 2014
                        </span>
                  </li>
                  <!-- /.timeline-label -->
                  <!-- timeline item -->
                  <li>
                    <i class="fa fa-camera bg-purple"></i>

                    <div class="timeline-item">
                      <span class="time"><i class="fa fa-clock-o"></i> 2 days ago</span>

                      <h3 class="timeline-header"><a href="#">Mina Lee</a> uploaded new photos</h3>

                      <div class="timeline-body">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                        <img src="http://placehold.it/150x100" alt="..." class="margin">
                      </div>
                    </div>
                  </li>
                  <!-- END timeline item -->
                  <li>
                    <i class="fa fa-clock-o bg-gray"></i>
                  </li>
                </ul>
              </div>
        ')
      )
    ),
    title = "Dashboard example"
  ),
  server = function(input, output) { }
)
```

You should see something like that:

<a href="images/dashboard_timeline.png"><img src="images/dashboard_timeline.png" width="auto" height="auto" alt="blank"></a>

Congratulations!!! Actually, I told you that (it's just my opinion), if you write a
shiny app, it is more consistent to write everything in R and not to mix HTML and R,
or at least not too much. Below, I explain you how to translate this HTML code
in R.

### Convert this code from HTML to R

For this part, I will take the following HTML code that corresponds to a social box 
from adminLTE2.

<a href="images/socialbox_example.png"><img src="images/socialbox_example.png" width="auto" height="auto" alt="blank"></a>

The associated HTML is located in the widgets.html file:

```{html}
<!-- /.col -->
        <div class="col-md-4">
          <!-- Widget: user widget style 1 -->
          <div class="box box-widget widget-user">
            <!-- Add the bg color to the header using any of the bg-* classes -->
            <div class="widget-user-header bg-aqua-active">
              <h3 class="widget-user-username">Alexander Pierce</h3>
              <h5 class="widget-user-desc">Founder &amp; CEO</h5>
            </div>
            <div class="widget-user-image">
              <img class="img-circle" src="../dist/img/user1-128x128.jpg" alt="User Avatar">
            </div>
            <div class="box-footer">
              <div class="row">
                <div class="col-sm-4 border-right">
                  <div class="description-block">
                    <h5 class="description-header">3,200</h5>
                    <span class="description-text">SALES</span>
                  </div>
                  <!-- /.description-block -->
                </div>
                <!-- /.col -->
                <div class="col-sm-4 border-right">
                  <div class="description-block">
                    <h5 class="description-header">13,000</h5>
                    <span class="description-text">FOLLOWERS</span>
                  </div>
                  <!-- /.description-block -->
                </div>
                <!-- /.col -->
                <div class="col-sm-4">
                  <div class="description-block">
                    <h5 class="description-header">35</h5>
                    <span class="description-text">PRODUCTS</span>
                  </div>
                  <!-- /.description-block -->
                </div>
                <!-- /.col -->
              </div>
              <!-- /.row -->
            </div>
          </div>
          <!-- /.widget-user -->
        </div>
        <!-- /.col -->
```

To convert it into R we will use the **tags** function from the htmltools package.
Each HTML5 tag will be preceded by tags such as `tags$h5("an example")` or
`tags$div(class = "col-sm-4")`. Anyway,
if you enter `tags$` in the console, a list of all valid tags will appear (and I have
no idea of the total number). 

The converted code will look like that:

```{r}
# col
tags$div(
  class = "col-md-4",
  # Widget: user widget style 1
  tags$div(
    class = "box box-widget widget-user",
    # Add the bg color to the header using any of the bg-* classes
    ## Box Header ##
    tags$div(
      class = "widget-user-header bg-aqua-active",
      tags$h3(class = "widget-user-username", "Alexander Pierce"),
      tags$h5(class = "widget-user-desc", "Founder at CEO")
    )
    ## Box Image ##
    tags$div(
      class = "widget-user-image",
      tags$img(class = "img-circle", src = "", alt = "User Avatar")
    ),
    ## Box Footer ##
    tags$div(
      class = "box-footer",
      tags$div(
        class = "row",
        # first column
        tags$div(
          class = "col-sm-4 border-right",
          tags$div(
            class = "description-block",
            tags$h5(class = "description-header", "3200"),
            tags$span(class = "description-text", "SALES")
          )
        ),
        # second column
        tags$div(
          class = "col-sm-4 border-right",
          tags$div(
            class = "description-block",
            tags$h5(class = "description-header", "13000"),
            tags$span(class = "description-text", "FOLLOWERS")
          )
        ),
        # third column
        tags$div(
          class = "col-sm-4 border-right",
          tags$div(
            class = "description-block",
            tags$h5(class = "description-header", "35"),
            tags$span(class = "description-text", "PRODUCTS")
          )
        )
      )
    )
  )
)
```

Having a try with our own basic shinydashboard template:

```{r}
library(shiny)
library(shinydashboard)
shinyApp(
  ui = dashboardPage(
    dashboardHeader(),
    dashboardSidebar(),
    dashboardBody(
      # col
      tags$div(
        class = "col-md-4",
        # Widget: user widget style 1
        tags$div(
          class = "box box-widget widget-user",
          # Add the bg color to the header using any of the bg-* classes
          ## Box Header ##
          tags$div(
            class = "widget-user-header bg-aqua-active",
            tags$h3(class = "widget-user-username", "Alexander Pierce"),
            tags$h5(class = "widget-user-desc", "Founder at CEO")
          ),
          ## Box Image ##
          tags$div(
            class = "widget-user-image",
            tags$img(class = "img-circle", src = "", alt = "User Avatar")
          ),
          ## Box Footer ##
          tags$div(
            class = "box-footer",
            tags$div(
              class = "row",
              # first column
              tags$div(
                class = "col-sm-4 border-right",
                tags$div(
                  class = "description-block",
                  tags$h5(class = "description-header", "3200"),
                  tags$span(class = "description-text", "SALES")
                )
              ),
              # second column
              tags$div(
                class = "col-sm-4 border-right",
                tags$div(
                  class = "description-block",
                  tags$h5(class = "description-header", "13000"),
                  tags$span(class = "description-text", "FOLLOWERS")
                )
              ),
              # third column
              tags$div(
                class = "col-sm-4 border-right",
                tags$div(
                  class = "description-block",
                  tags$h5(class = "description-header", "35"),
                  tags$span(class = "description-text", "PRODUCTS")
                )
              )
            )
          )
        )
      )
    ),
    title = "Dashboard example"
  ),
  server = function(input, output) { }
)
```

Notice that the image is not display because I did not provide any source
`tags$img(class = "img-circle", src = "", alt = "User Avatar")`. Up to you to provide
your own image that should by in the **/www** directory of your application. 
Of course, it would be better if you wrap this custom R code in your own function like that:

```{r}
my_tiny_socialbox <- function(username = "Paul", userposition = "looking for job",
                              sales = NULL, followers = NULL, products = NULL) {
                              
  # check some inputs
  stopifnot(is.numeric(sales))
  stopifnot(is.numeric(followers))
  stopifnot(is.numeric(products))
  
  # the box tags
  withTags(
    # col
    div(
      class = "col-md-4",
      # Widget: user widget style 1
      div(
        class = "box box-widget widget-user",
        # Add the bg color to the header using any of the bg-* classes
        ## Box Header ##
        div(
          class = "widget-user-header bg-aqua-active",
          h3(class = "widget-user-username", username),
          h5(class = "widget-user-desc", userposition)
        ),
        ## Box Image ##
        div(
          class = "widget-user-image",
          img(class = "img-circle", src = "", alt = "User Avatar")
        ),
        ## Box Footer ##
        div(
          class = "box-footer",
          div(
            class = "row",
            # first column
            div(
              class = "col-sm-4 border-right",
              div(
                class = "description-block",
                h5(class = "description-header", sales),
                span(class = "description-text", "SALES")
              )
            ),
            # second column
            div(
              class = "col-sm-4 border-right",
              div(
                class = "description-block",
                h5(class = "description-header", followers),
                span(class = "description-text", "FOLLOWERS")
              )
            ),
            # third column
            div(
              class = "col-sm-4 border-right",
              div(
                class = "description-block",
                h5(class = "description-header", products),
                span(class = "description-text", "PRODUCTS")
              )
            )
          )
        )
      )
    )
  )
}
```

where you would have some fields such as username, userposition, sales, followers and products.
Notice that I use the **withTags** function that enables me to remove `tags$`.

```{r}
# create your custom box
my_tiny_socialbox <- function(username = "Paul", userposition = "looking for job",
                              sales = NULL, followers = NULL, products = NULL) {
                              
  # check some inputs
  stopifnot(is.numeric(sales))
  stopifnot(is.numeric(followers))
  stopifnot(is.numeric(products))
  
  # the box tags
  withTags(
    # col
    div(
      class = "col-md-4",
      # Widget: user widget style 1
      div(
        class = "box box-widget widget-user",
        # Add the bg color to the header using any of the bg-* classes
        ## Box Header ##
        div(
          class = "widget-user-header bg-aqua-active",
          h3(class = "widget-user-username", username),
          h5(class = "widget-user-desc", userposition)
        ),
        ## Box Image ##
        div(
          class = "widget-user-image",
          img(class = "img-circle", src = "", alt = "User Avatar")
        ),
        ## Box Footer ##
        div(
          class = "box-footer",
          div(
            class = "row",
            # first column
            div(
              class = "col-sm-4 border-right",
              div(
                class = "description-block",
                h5(class = "description-header", sales),
                span(class = "description-text", "SALES")
              )
            ),
            # second column
            div(
              class = "col-sm-4 border-right",
              div(
                class = "description-block",
                h5(class = "description-header", followers),
                span(class = "description-text", "FOLLOWERS")
              )
            ),
            # third column
            div(
              class = "col-sm-4 border-right",
              div(
                class = "description-block",
                h5(class = "description-header", products),
                span(class = "description-text", "PRODUCTS")
              )
            )
          )
        )
      )
    )
  )
}

# launch the dashboard with our simple custom function
library(shiny)
library(shinydashboard)
shinyApp(
  ui = dashboardPage(
    dashboardHeader(),
    dashboardSidebar(),
    dashboardBody(
      my_tiny_socialbox(username = "Paul", userposition = "looking for job",
                        sales = 150, followers = 1200, products = 3)
    ),
    title = "Dashboard example"
  ),
  server = function(input, output) { }
)

```

Congratulations!

<a href="images/dashboard_socialbox.png"><img src="images/dashboard_socialbox.png" width="auto" height="auto" alt="blank"></a>

From now, you should be able to integrate any boostrap 3 compatible objects in your
shinydashboards. 

A better example of what you could achieve is [here](https://dgranjon.shinyapps.io/myshinycv/)

<a href="images/shinyCV_newdesign.png"><img src="images/shinyCV_newdesign.png" width="auto" height="auto" alt="blank"></a>

Interestingly, you will also be able to build closable boxes (which is not part of shinydashboard, as far as I know ...)

## Some improved version of shinydashboard

### ygdashboard

A very interesting feature of adminLTE2 is the right sidebar, in which you can
put custom widgets. Be careful, there seems to be a limit in the number of panels
(up to 5).

<a href="images/right_sidebar_example.png"><img src="images/right_sidebar_example.png" width="100px" height="300px" alt="blank"></a>

A package called [ygdashboard](https://github.com/gyang274/ygdashboard/tree/master/R) 
integrates this option to shinydashboard. However, you cannot use shinydashboard and
ygdashboard at the same time.

### gentelella dashboard

A version very similar to adminLTE2. By chance, a shiny implementation is available
[here](http://code.markedmondson.me/gentelellaShiny/). 

<a href="images/shiny_gentelella.png"><img src="images/shiny_gentelella.png" width="auto" height="auto" alt="blank"></a>

This template looks really promising!

### Material dashboards

**Coming soon**


## Last Notes

A new version of adminLTE, namely [adminLTE3](https://twitter.com/almsaeedstudio?lang=fr) 
is coming soon, but relies on [boostrap 4](https://github.com/rstudio/shiny/issues/1604) 
(which is not natevely supported by shiny).
There exists a package called [dull](https://github.com/nteetor/dull#readme), which 
is work in progress and I hope it will work very soon, to make possible integrating
bootstrap 4 elements to shiny. 