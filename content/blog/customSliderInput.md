+++
  title = "How to customize shiny sliderInput"
  date = "2017-11-11"
+++
    
Sometimes, you might get bored by the design of the **sliders** in Shiny. This input widget
uses the [Ion.Rangeslider](http://ionden.com/a/plugins/ion.rangeSlider/en.html) js library.
In the advanced [section](http://ionden.com/a/plugins/ion.rangeSlider/demo_advanced.html),
you can notice that 5 themes are actually available. I really like the flat ui skin.
In this tutorial, I will describe two ways to install new themes for your shiny
sliders (a third way is in development)


## Create your own input-slider.R function

At the moment, this is my favorite option since it does not alter the shiny
package code directly. 

1. In the root folder of your shiny app, create a function called input-slider.R.
Actually, the name is not so important (you could even call it slidercustom.R or
whatever...)
2. Copy and paste the following code in your new file (this is a simple copy
of the original input-slider.R of the shiny package, available 
[here](https://github.com/rstudio/shiny/blob/master/R/input-slider.R)):

```
#' Slider Input Widget
#'
#' Constructs a slider widget to select a numeric value from a range.
#'
#' @inheritParams textInput
#' @param min The minimum value (inclusive) that can be selected.
#' @param max The maximum value (inclusive) that can be selected.
#' @param value The initial value of the slider. A numeric vector of length one
#'   will create a regular slider; a numeric vector of length two will create a
#'   double-ended range slider. A warning will be issued if the value doesn't
#'   fit between \code{min} and \code{max}.
#' @param step Specifies the interval between each selectable value on the
#'   slider (if \code{NULL}, a heuristic is used to determine the step size). If
#'   the values are dates, \code{step} is in days; if the values are times
#'   (POSIXt), \code{step} is in seconds.
#' @param round \code{TRUE} to round all values to the nearest integer;
#'   \code{FALSE} if no rounding is desired; or an integer to round to that
#'   number of digits (for example, 1 will round to the nearest 10, and -2 will
#'   round to the nearest .01). Any rounding will be applied after snapping to
#'   the nearest step.
#' @param format Deprecated.
#' @param locale Deprecated.
#' @param ticks \code{FALSE} to hide tick marks, \code{TRUE} to show them
#'   according to some simple heuristics.
#' @param animate \code{TRUE} to show simple animation controls with default
#'   settings; \code{FALSE} not to; or a custom settings list, such as those
#'   created using \code{\link{animationOptions}}.
#' @param sep Separator between thousands places in numbers.
#' @param pre A prefix string to put in front of the value.
#' @param post A suffix string to put after the value.
#' @param dragRange This option is used only if it is a range slider (with two
#'   values). If \code{TRUE} (the default), the range can be dragged. In other
#'   words, the min and max can be dragged together. If \code{FALSE}, the range
#'   cannot be dragged.
#' @param timeFormat Only used if the values are Date or POSIXt objects. A time
#'   format string, to be passed to the Javascript strftime library. See
#'   \url{https://github.com/samsonjs/strftime} for more details. The allowed
#'   format specifications are very similar, but not identical, to those for R's
#'   \code{\link[base]{strftime}} function. For Dates, the default is \code{"\%F"}
#'   (like \code{"2015-07-01"}), and for POSIXt, the default is \code{"\%F \%T"}
#'   (like \code{"2015-07-01 15:32:10"}).
#' @param timezone Only used if the values are POSIXt objects. A string
#'   specifying the time zone offset for the displayed times, in the format
#'   \code{"+HHMM"} or \code{"-HHMM"}. If \code{NULL} (the default), times will
#'   be displayed in the browser's time zone. The value \code{"+0000"} will
#'   result in UTC time.
#' @inheritParams selectizeInput
#' @family input elements
#' @seealso \code{\link{updateSliderInput}}
#'
#' @examples
#' ## Only run examples in interactive R sessions
#' if (interactive()) {
#' options(device.ask.default = FALSE)
#'
#' ui <- fluidPage(
#'   sliderInput("obs", "Number of observations:",
#'     min = 0, max = 1000, value = 500
#'   ),
#'   plotOutput("distPlot")
#' )
#'
#' # Server logic
#' server <- function(input, output) {
#'   output$distPlot <- renderPlot({
#'     hist(rnorm(input$obs))
#'   })
#' }
#'
#' # Complete app with UI and server components
#' shinyApp(ui, server)
#' }
#' @export
sliderInput <- function(inputId, label, min, max, value, step = NULL,
                        round = FALSE, format = NULL, locale = NULL,
                        ticks = TRUE, animate = FALSE, width = NULL, sep = ",",
                        pre = NULL, post = NULL, timeFormat = NULL,
                        timezone = NULL, dragRange = TRUE)
{
  if (!missing(format)) {
    shinyDeprecated(msg = "The `format` argument to sliderInput is deprecated. Use `sep`, `pre`, and `post` instead.",
                    version = "0.10.2.2")
  }
  if (!missing(locale)) {
    shinyDeprecated(msg = "The `locale` argument to sliderInput is deprecated. Use `sep`, `pre`, and `post` instead.",
                    version = "0.10.2.2")
  }

  if (inherits(min, "Date")) {
    if (!inherits(max, "Date") || !inherits(value, "Date"))
      stop("`min`, `max`, and `value must all be Date or non-Date objects")
    dataType <- "date"

    if (is.null(timeFormat))
      timeFormat <- "%F"

  } else if (inherits(min, "POSIXt")) {
    if (!inherits(max, "POSIXt") || !inherits(value, "POSIXt"))
      stop("`min`, `max`, and `value must all be POSIXt or non-POSIXt objects")
    dataType <- "datetime"

    if (is.null(timeFormat))
      timeFormat <- "%F %T"

  } else {
    dataType <- "number"
  }

  # Restore bookmarked values here, after doing the type checking, because the
  # restored value will be a character vector instead of Date or POSIXct, and we can do
  # the conversion to correct type next.
  value <- restoreInput(id = inputId, default = value)

  if (is.character(value)) {
    # If we got here, the value was restored from a URL-encoded bookmark.
    if (dataType == "date") {
      value <- as.Date(value, format = "%Y-%m-%d")
    } else if (dataType == "datetime") {
      # Date-times will have a format like "2018-02-28T03:46:26Z"
      value <- as.POSIXct(value, format = "%Y-%m-%dT%H:%M:%SZ", tz = "UTC")
    }
  }

  step <- findStepSize(min, max, step)

  if (dataType %in% c("date", "datetime")) {
    # For Dates, this conversion uses midnight on that date in UTC
    to_ms <- function(x) 1000 * as.numeric(as.POSIXct(x))

    # Convert values to milliseconds since epoch (this is the value JS uses)
    # Find step size in ms
    step  <- to_ms(max) - to_ms(max - step)
    min   <- to_ms(min)
    max   <- to_ms(max)
    value <- to_ms(value)
  }

  range <- max - min

  # Try to get a sane number of tick marks
  if (ticks) {
    n_steps <- range / step

    # Make sure there are <= 10 steps.
    # n_ticks can be a noninteger, which is good when the range is not an
    # integer multiple of the step size, e.g., min=1, max=10, step=4
    scale_factor <- ceiling(n_steps / 10)
    n_ticks <- n_steps / scale_factor

  } else {
    n_ticks <- NULL
  }

  sliderProps <- dropNulls(list(
    class = "js-range-slider",
    id = inputId,
    `data-type` = if (length(value) > 1) "double",
    `data-min` = formatNoSci(min),
    `data-max` = formatNoSci(max),
    `data-from` = formatNoSci(value[1]),
    `data-to` = if (length(value) > 1) formatNoSci(value[2]),
    `data-step` = formatNoSci(step),
    `data-grid` = ticks,
    `data-grid-num` = n_ticks,
    `data-grid-snap` = FALSE,
    `data-prettify-separator` = sep,
    `data-prettify-enabled` = (sep != ""),
    `data-prefix` = pre,
    `data-postfix` = post,
    `data-keyboard` = TRUE,
    # This value is only relevant for range sliders; for non-range sliders it
    # causes problems since ion.RangeSlider 2.1.2 (issue #1605).
    `data-drag-interval` = if (length(value) > 1) dragRange,
    # The following are ignored by the ion.rangeSlider, but are used by Shiny.
    `data-data-type` = dataType,
    `data-time-format` = timeFormat,
    `data-timezone` = timezone
  ))

  # Replace any TRUE and FALSE with "true" and "false"
  sliderProps <- lapply(sliderProps, function(x) {
    if (identical(x, TRUE)) "true"
    else if (identical(x, FALSE)) "false"
    else x
  })

  sliderTag <- div(class = "form-group shiny-input-container",
    style = if (!is.null(width)) paste0("width: ", validateCssUnit(width), ";"),
    if (!is.null(label)) controlLabel(inputId, label),
    do.call(tags$input, sliderProps)
  )

  # Add animation buttons
  if (identical(animate, TRUE))
    animate <- animationOptions()

  if (!is.null(animate) && !identical(animate, FALSE)) {
    if (is.null(animate$playButton))
      animate$playButton <- icon('play', lib = 'glyphicon')
    if (is.null(animate$pauseButton))
      animate$pauseButton <- icon('pause', lib = 'glyphicon')

    sliderTag <- tagAppendChild(
      sliderTag,
      tags$div(class='slider-animate-container',
        tags$a(href='#',
          class='slider-animate-button',
          'data-target-id'=inputId,
          'data-interval'=animate$interval,
          'data-loop'=animate$loop,
          span(class = 'play', animate$playButton),
          span(class = 'pause', animate$pauseButton)
        )
      )
    )
  }

  dep <- list(
    htmlDependency("ionrangeslider", "2.1.6", c(href="shared/ionrangeslider"),
      script = "js/ion.rangeSlider.min.js",
      # ion.rangeSlider also needs normalize.css, which is already included in
      # Bootstrap.
      stylesheet = c("css/ion.rangeSlider.css",
                     "css/ion.rangeSlider.skinShiny.css")
    ),
    htmlDependency("strftime", "0.9.2", c(href="shared/strftime"),
      script = "strftime-min.js"
    )
  )

  attachDependencies(sliderTag, dep)
}

hasDecimals <- function(value) {
  truncatedValue <- round(value)
  return (!identical(value, truncatedValue))
}


# If step is NULL, use heuristic to set the step size.
findStepSize <- function(min, max, step) {
  if (!is.null(step)) return(step)

  range <- max - min
  # If short range or decimals, use continuous decimal with ~100 points
  if (range < 2 || hasDecimals(min) || hasDecimals(max)) {
    # Workaround for rounding errors (#1006): the intervals between the items
    # returned by pretty() can have rounding errors. To avoid this, we'll use
    # pretty() to find the min, max, and number of steps, and then use those
    # values to calculate the step size.
    pretty_steps <- pretty(c(min, max), n = 100)
    n_steps <- length(pretty_steps) - 1
    (max(pretty_steps) - min(pretty_steps)) / n_steps

  } else {
    1
  }
}


#' @rdname sliderInput
#'
#' @param interval The interval, in milliseconds, between each animation step.
#' @param loop \code{TRUE} to automatically restart the animation when it
#'   reaches the end.
#' @param playButton Specifies the appearance of the play button. Valid values
#'   are a one-element character vector (for a simple text label), an HTML tag
#'   or list of tags (using \code{\link{tag}} and friends), or raw HTML (using
#'   \code{\link{HTML}}).
#' @param pauseButton Similar to \code{playButton}, but for the pause button.
#' @export
animationOptions <- function(interval=1000,
                             loop=FALSE,
                             playButton=NULL,
                             pauseButton=NULL) {
  list(interval=interval,
       loop=loop,
       playButton=playButton,
       pauseButton=pauseButton)
}
```

3. Choose the skin you want among: Shiny (original), Flat, Modern, HTML5,
Simple, Round, Square and Nice. All the css files supported by shiny are
[here](https://github.com/rstudio/shiny/tree/master/inst/www/shared/ionrangeslider/css) (shiny/inst/www/shared/ionrangeslider/css/).

4. Look at the following line in the input-slider.R code:

```
dep <- list(
    htmlDependency("ionrangeslider", "2.1.6", c(href="shared/ionrangeslider"),
      script = "js/ion.rangeSlider.min.js",
      # ion.rangeSlider also needs normalize.css, which is already included in
      # Bootstrap.
      stylesheet = c("css/ion.rangeSlider.css",
                     "css/ion.rangeSlider.skinShiny.css")
    ),
    htmlDependency("strftime", "0.9.2", c(href="shared/strftime"),
      script = "strftime-min.js"
    )
  )
```
and replace `"css/ion.rangeSlider.skinShiny.css"` by `"css/ion.rangeSlider.skinFlat.css"`
if you want to use the Flat design, and so on...

5. Very important: with this method, you **cannot use 2 different skins** at the same 
time. It means that if you have the Flat skin for one sliderInput, you cannot
use the Modern skin for your second slider. 

6. This is unfortunate and I will investigate how to solve this issue. 
By the way, the Flat design is amazing.

<div class = "row">
<div class="col-sm-6>
<a href="images/sliderFlat.png"><img src="images/sliderFlat.png" width="200px" height="100px" alt="blank"></a>
</div>
<div class="col-sm-6>
<a href="images/sliderModern.png"><img src="images/sliderModern.png" width="200px" height="100px" alt="blank"></a>
</div>
</div>
<div class = "row">
<div class="col-sm-6>
<a href="images/sliderSimple.png"><img src="images/sliderSimple.png" width="200px" height="100px" alt="blank"></a>
</div>
<div class="col-sm-6>
<a href="images/sliderNice.png"><img src="images/sliderNice.png" width="200px" height="100px" alt="blank"></a>
</div>
</div>
<div class = "row">
<div class="col-sm-6>
<a href="images/sliderRound.png"><img src="images/sliderRound.png" width="200px" height="100px" alt="blank"></a>
</div>
<div class="col-sm-6>
<a href="images/sliderSquare.png"><img src="images/sliderSquare.png" width="200px" height="100px" alt="blank"></a>
</div>
</div>

## Modify the shiny package code

This is not the cleanest way to change the slider themes but **it works**! 
Moreover, you will not need a custom input-slider.R in your shiny app folder 
(see the previous section).

1. First of all, you have to download shiny from **source** in order to access some
files required for this customization. In **Rstudio** console type:

```
install.packages("shiny", type = "source")
```

2. A link to the downloaded folder will be given. Then open a **terminal** and go to:
```sh
$ cd/folder_where_sources_are_downloaded
```
The corresponding folder contains a **tar.gz** file that you will have to extract.
Name the extracted file *shiny_custom*.
Besides, you can make a copy of the original shiny folder on your desktop. 
Anyway, if things are going wrong, you can always reinstall shiny: `install.packages("shiny")`.

3. Open the *shiny* folder containing sources. In the *R* subfolder, open *input-slider.R*.
Notice lines **220-231**:

```
dep <- list(
    htmlDependency("ionrangeslider", "2.1.6", c(href="shared/ionrangeslider"),
      script = "js/ion.rangeSlider.min.js",
      # ion.rangeSlider also needs normalize.css, which is already included in
      # Bootstrap.
      stylesheet = c("css/ion.rangeSlider.css",
                     "css/ion.rangeSlider.skinShiny.css")
    ),
    htmlDependency("strftime", "0.9.2", c(href="shared/strftime"),
      script = "strftime-min.js"
    )
  )
```
You will have to change this part `css/ion.rangeSlider.skinShiny.css`.

4. To choose a Ion.Rangeslider theme, you will find the list of all supported
css skins [here](https://github.com/rstudio/shiny/tree/master/inst/www/shared/ionrangeslider/css). 

5. Go back to the *input-slider.R* file of your shiny source, and replace `css/ion.rangeSlider.skinShiny.css`
by `ion.rangeSlider.skinFlat.css` if you want the Flat design (you are free to select another one).

6. You need now to make an archive of the modified shiny folder (containing the custom
slider skin). Open a **terminal** and go to the directory of your new shiny folder:
```sh
$ tar -zcvf shiny_1.0.5.tar.gz shiny_custom
```

7. Delete the official shiny package folder (it is stored in the library folder of R). 
On Mac, it is in:

```sh
$ cd /Library/Frameworks/R.framework/Versions/3.3/Resources/library/
```

8. Open a terminal and go back in the directory containing your own modified shiny package. Then run:

```sh
$ R CMD INSTALL shiny_1.0.5.tar.gz
```

This will install your updates. If everything is fine, your sliders should
be different from that used by default in shiny. Be careful that each future update of shiny
will **erase** your modifications. Up to you to **save** this work.

<a href="images/flat_sliders.png"><img src="images/flat_sliders.png" width="200px" height="350px" alt="blank"></a>