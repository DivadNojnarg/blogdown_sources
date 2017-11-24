+++
  title = "Extend shinyWidgets: knobInput"
  date = "2017-11-21"
+++

## Introduction

[shiny](https://shiny.rstudio.com) basic package contains several widgets, available
[here](https://shiny.rstudio.com/gallery/widget-gallery.html). This database is
continuously extended through the [shinyWidget](https://github.com/dreamRs/shinyWidgets) 
package developed by the [dreamRs](https://www.dreamrs.fr) team. Here I present how we
add a new knobInput widget to shinyWidget, with Victor Perrier from the dreamRs team.

I always wanted to play with some knobs in shiny, similarly as what we do with sliders.
However, in spite of some research, I found nothing already implemented. Later, I discovered
this js library called [jQuery knob](https://github.com/aterrien). In parallel, I started to read some tutorials
on *"how to extend shiny"* and decided to create a new RStudio project to include this
new widget. Since I am not a expert in javascript and jQuery, I asked a [question](https://stackoverflow.com/questions/47261702/create-a-knobinput-widget-in-shiny-app) 
on stackoverflow and Victor Perrier nicely solved my problem. Then, we both decided to include
this new widget in the shinyWidget package. Below is a short explanation on how to build such a custom input,
which is not trivial but not so hard. I hope it will be useful for you, who are reading this
blog post. 

Here is the final result on shinyapps.io: [shinyknob](https://dgranjon.shinyapps.io/knobinput_example/)

<iframe src="https://dgranjon.shinyapps.io/knobinput_example/" style="width: 700px; height: 700px; border: none; overflow: hidden;"></iframe>

## Tutorial

The idea is simple:

1. First, you create the file containing the new widget: *input-knob.R*, 
because it is supposed to be a knob (very original). It is widely inspired
on what you can find on the shiny github R [folder](https://github.com/rstudio/shiny/tree/master/R).
Below, I quickly describe its content:

```
# Knob Input
knobInput <- function(inputId, label, value, min = 0, max = 100, step = 1,
                      angleOffset = 0, angleArc = 360, cursor = FALSE,
                      thickness = NULL, lineCap = c("default", "round"), displayInput = TRUE,
                      displayPrevious = FALSE, rotation = c("clockwise", "anticlockwise"),
                      fgColor = NULL, inputColor = NULL, bgColor = NULL,
                      readOnly = FALSE, skin = NULL, width = NULL, height = NULL) {
  value <- shiny::restoreInput(id = inputId, default = value)
  lineCap <- match.arg(lineCap)
  rotation <- match.arg(rotation)
  knobParams <- dropNulls(list(
    id = inputId, type = "text", class = "knob-input", 
    value = value, `data-value` = value, `data-skin` = skin,
    `data-min` = min, `data-max` = max, `data-step` = step, 
    `data-angleoffset` = angleOffset, `data-anglearc` = angleArc,
    `data-displayprevious` = displayPrevious, `data-thickness` = thickness,
    `data-displayinput` = displayInput, `data-linecap` = lineCap,
    `data-fgcolor` = fgColor, `data-inputcolor` = inputColor,
    `data-bgcolor` = bgColor, `data-cursor` = cursor,
    `data-rotation` = rotation, `data-readonly` = readOnly,
    `data-width` = width, `data-height` = height
  ))
  knobParams <- lapply(knobParams, function(x) {
    if (identical(x, TRUE))
      "true"
    else if (identical(x, FALSE))
      "false"
    else x
  })
  inputTag <- do.call(htmltools::tags$input, knobParams)
  knobInputTag <- htmltools::tags$div(
    class = "form-group shiny-input-container",
    style = if (!is.null(width)) paste0("width: ", htmltools::validateCssUnit(width), ";"),
    if (!is.null(label)) htmltools::tags$label(`for` = inputId, label),
    if (!is.null(label)) htmltools::tags$br(),
    inputTag
  )
  dep <- htmltools::htmlDependency(
    name = "jquery-knob", version = "1.2.13",
    src = c(href = "jquery-knob"),
    script = c("jquery.knob.min.js",
               "knob-input-binding.js")
  )
  htmltools::attachDependencies(knobInputTag, dep)
  
  # Change the value of a knob input on the client
  updateKnobInput <- function(session, inputId, label = NULL, value = NULL, readOnly = NULL) {
    message <- dropNulls(list(label = label, value = value, readOnly = readOnly))
    session$sendInputMessage(inputId, message)
  }
  
  dropNulls <- function(x) {
    x[!vapply(x, is.null, FUN.VALUE = logical(1))]
}
```

First of all, all the options available in jQuery knob are included in the function
arguments. Thus, I highly recommend you to check the corresponding documentation.
The value is recovered using shiny's **restoreInput** function. linecap and rotation
can have 2 different values. Therefore, the **match.arg** function allows to select the values.
All parameters passed in the function are stored in a list called knobParams. Importantly, 
as shown in jQuery knob demo page, type is set as "text". We choose to name the class "knob-input",
instead of "dial" (see the [documentation](http://anthonyterrien.com/knob/)), without any consequences. 
Null values are not considered (**dropNulls** function). Since we use a js library, TRUE and FALSE
cannot be recognized, unless they are converted in true and false, respectively. 
This is done, using the **lapply** and **identical** functions. Our knobParam widget is wrapped in an input div (`tags$input`),
as also shown in jQuery knob documentation:

```HTML

<input type="text" class="dial" data-min="-50" data-max="50">

```

We store all dependencies in the dep object, via
**htmlDependency** function from htmltools package. Notice that in the shinyWidget 
[code](https://github.com/dreamRs/shinyWidgets/blob/master/R/input-knob.R), this part is
slightly modified, since files are organized in another way. We also create an **updateKnobInput** function,
which will allow the user to update the value and label of the knob (readOnly does not
work at the moment). **dropNulls** is defined below but could be written in a utils.R file, for instance. 

Additionally, your /www folder should contain both *jquery.knob.min.js* and *knob-input-binding.js*,
the later is discussed below.

2. Then, you need to create the related input-binding. This file should be put in the
/www folder of the widget. To summarise, the input-binding will make the link between
shiny and your new widget (see [here](https://shiny.rstudio.com/articles/building-inputs.html)).

```javascript

var exportsKnob = window.Shiny = window.Shiny || {};
var $escapeKnob = exportsKnob.$escape = function(val) {
    return val.replace(/([!"#$%&'()*+,.\/:;<=>?@\[\\\]^`{|}~])/g, '\\$1');
};



function tron_skin() {

    // "tron" case
    if (this.$.data('skin') == 'tron') {
        this.cursorExt = 0.3;
        var a = this.arc(this.cv) // Arc
            ,
            pa // Previous arc
            , r = 1;
        this.g.lineWidth = this.lineWidth;
        if (this.o.displayPrevious) {
            pa = this.arc(this.v);
            this.g.beginPath();
            this.g.strokeStyle = this.pColor;
            this.g.arc(this.xy, this.xy, this.radius - this.lineWidth, pa.s, pa.e, pa.d);
            this.g.stroke();
        }
        this.g.beginPath();
        this.g.strokeStyle = r ? this.o.fgColor : this.fgColor;
        this.g.arc(this.xy, this.xy, this.radius - this.lineWidth, a.s, a.e, a.d);
        this.g.stroke();
        this.g.lineWidth = 2;
        this.g.beginPath();
        this.g.strokeStyle = this.o.fgColor;
        this.g.arc(this.xy, this.xy, this.radius - this.lineWidth + 1 + this.lineWidth * 2 / 3, 0, 2 * Math.PI, false);
        this.g.stroke();
        return false;
    }
}


// Knob input binding

var knobInputBinding = new Shiny.InputBinding();


// An input binding must implement these methods
$.extend(knobInputBinding, {

    // This returns a jQuery object with the DOM element
    find: function(scope) {
        return $(scope).find('.knob-input');
    },

    // this method will be called on initialisation
    initialize: function(el) {

        // extract the value from el
        // note here our knobInput does not yet exist
        var value = $(el).data("value");

        // initialize our knob based on the extracted state
        el.value = value;

        // Initialize knob
        $(el).knob({draw: tron_skin});
    },

    // Given the DOM element for the input, return the value
    getValue: function(el) {
        return parseFloat(el.value);
    },

    // Set up the event listeners so that interactions with the
    // input will result in data being sent to server.
    // callback is a function that queues data to be sent to
    // the server.
    subscribe: function(el, callback) {
        $(el).on('keyup.knobInputBinding', function(event) {
            callback(true);
            // When called with true, it will use the rate policy,
            // which in this case is to debounce at 500ms.
        });

        $(el).trigger('configure', {
            'change': function(v) {
                callback(false);
            },
            'release' : function (v) { 
                callback(false);
            }
        });
        
        $(el).on('change.knobInputBinding', function(event) {
            callback(false);
            // When called with false, it will NOT use the rate policy,
            // so changes will be sent immediately
        });
    },

    // Remove the event listeners
    unsubscribe: function(el) {
        $(el).off('.knobInputBinding');
    },

    // Receive messages from the server.
    // Messages sent by updateknobInput() are received by this function.
    receiveMessage: function(el, data) {
        if (data.hasOwnProperty('value')) {
            $(el)
                .val(data.value)
                .trigger('change');
        }

        if (data.hasOwnProperty('readOnly')) {
            $(el).trigger(
                'configure', {
                    "readOnly": data.readOnly
                }
            );
        }


        if (data.hasOwnProperty('label'))
            $(el).parent().parent().find('label[for="' + $escapeKnob(el.id) + '"]').text(data.label);

        $(el).trigger('change');
    },

    // This returns a full description of the input's state.
    // Note that some inputs may be too complex for a full description of the
    // state to be feasible.
    getState: function(el) {
        return {
            label: $(el).parent().parent().find('label[for="' + $escapeKnob(el.id) + '"]').text(),
            value: el.value
        };
    },

    // The input rate limiting policy
    getRatePolicy: function() {
        return {
            // Can be 'debounce' or 'throttle'
            policy: 'debounce',
            delay: 500
        };
    }

});

Shiny.inputBindings.register(knobInputBinding, 'shiny.knobInput');

```

The first part of this binding contain a javascript code when the "tron" skin is used.
It is just copied and paste from jQuery knob library. Then, we define the input binding as follows:

- we declare a new variable (not surprisingly "knobInputBinding").
- we call the **find** method using the class "knob-input", previously defined in our
*input-knob.R* file. Crucial step!
- the **initialize** function creates the knob using .knob() (see jQuery knob), where we set 
the value of the widget.
- the method **getValue** returns the value of your widget(s).
- **subscribe** method: one of the most important feature. Without this part, 
the value of your widget will not be updated in shiny when you change it. We can find
several methods inside:

```javascript
$(el).on('keyup.knobInputBinding', function(event) {
            callback(true);
            // When called with true, it will use the rate policy,
            // which in this case is to debounce at 500ms.
        });
```

**keyup** method is related to events on the user keyboard (especially multidirectional arrows).

```javascript
$(el).trigger('configure', {
            'change': function(v) {
                callback(false);
            },
            'release' : function (v) { 
                callback(false);
            }
        });
```
The **configure** method enables to dynamically configure the widget.
The **change** method is used when the value of the input changes. Without **release**, 
scrolling with the mouse wheel will not update the value in shiny. We found relevant
to set this kind of behavious.

Besides, the **callback** function is useful to set a rate policy for the widget (see below).

- **getRatePolicy** will avoid to send to much requests to the server. For example,
let's say the user wants to move the knob from 1 to 100. Without rate policy, each change
from 1 to 100 would be sent to the server, which is a bit ridiculous. Rate policy is called
when callback has "true" as argument.
- we register the input binding with the **register** function. 

3. Finally, we create a short demonstration of this widget.

```

# knob --------------------------------------------------------------------

# Package
library("shiny")

# Fun
source("input-knob.R")

# HTML dep
shiny::addResourcePath('jquery-knob', file.path(getwd(), "www"))




# ui ---- 
ui <- fluidPage(
  tags$h1("knob input examples"), 
  tags$h4("made by Victor Perrier (shinyWidgets) and David Granjon"),
  tags$h4("based on", a("jQuery knob", href = "https://github.com/aterrien/jQuery-Knob")),
  br(),
  
  fluidRow(
    
    column(
      width = 4,
      knobInput(inputId = "knob1", label = "Default:", value = 10),
      verbatimTextOutput(outputId = "res1"),
      
      knobInput(inputId = "knob2", label = "Color:", value = 1, fgColor = "#D9534F", displayInput = FALSE),
      verbatimTextOutput(outputId = "res2"),
      
      knobInput(inputId = "knob3", label = "Read only:", value = 65, readOnly = TRUE),
      verbatimTextOutput(outputId = "res3")
    ),
    
    column(
      width = 4,
      knobInput(inputId = "knob6", label = "Cursor mode:", value = 50, thickness = 0.3, cursor = TRUE, width = 150, height = 150), # must specify width
      verbatimTextOutput(outputId = "res6"),
      
      knobInput(inputId = "knob7", label = "Display previous:", value = 50, min = -100, displayPrevious = TRUE, fgColor = "#428BCA", inputColor = "#428BCA"),
      verbatimTextOutput(outputId = "res7"),
      
      tags$div(
        style = "background-color: #000;",
        knobInput(inputId = "knob8", label = tags$span("Tron skin:", style = "color: #FFF;"), value = 50, width = 75, fgColor = "#ffec03", inputColor = "#ffec03", skin = "tron")
      ),
      verbatimTextOutput(outputId = "res8")
    ),
    
    column(
      width = 4,
      knobInput(inputId = "knob11", label = "Angle offset:", value = 75, angleOffset = 90, lineCap = "round"),
      verbatimTextOutput(outputId = "res11"),
      
      knobInput(inputId = "knob12", label = "Angle offset and arc:", value = 50, angleOffset = -125, angleArc = 250),
      verbatimTextOutput(outputId = "res12"),
      
      knobInput(inputId = "knob13", label = "4-digit, step 0.1:", value = 0, min = -1000, max = 1000, step = 0.1, displayPrevious = TRUE),
      verbatimTextOutput(outputId = "res13")
    )
    
  )
)


# server ----
server <- function(input, output, session) {

  lapply(1:13, function(x) {
    output[[paste0("res", x)]] <- renderPrint(input[[paste0("knob", x)]])
  })
  
}


# app ----
shinyApp(ui = ui, server = server)

```

Basically, there are 4 ways you can change the input: 

- using multidirectional arrows
- entering text in the input field (when it is present, ie displayInput is set to TRUE)
- dragging the knob with the mouse
- scrolling with the mouse wheel.

For the moment, the readOnly argument does not work with the updateKnobInput function.