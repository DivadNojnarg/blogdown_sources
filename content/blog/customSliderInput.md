+++
  title = "How to customize shiny sliderInput"
  date = "2017-11-11"
+++
    
Sometimes, you might get bored by the design of the **sliders** in Shiny. This input widget
uses the [Ion.Rangeslider](http://ionden.com/a/plugins/ion.rangeSlider/en.html) js library.
In the advanced [section](http://ionden.com/a/plugins/ion.rangeSlider/demo_advanced.html),
you can notice that 5 themes are actually available. I really like the flat ui skin.
In this tutorial, I will describe one way to install new themes for your shiny
sliders. 

* First of all, you have to download shiny from **source** in order to access some
files required for this customization. In **Rstudio** console type:

```R
install.packages("shiny", type = "source")
```

* A link to the downloaded folder will be given. Then open a **terminal** and go to:
```R
$ cd/folder_where_sources_are_downloaded
```
The corresponding folder contains a **tar.gz** file that you will have to extract.
Name the extracted file *shiny_custom*.
Besides, you can make a copy of the original shiny folder on your desktop. 
Anyway, if things are going wrong, you can always reinstall shiny: `install.packages("shiny")`.

* Open the *shiny* folder containing sources. In the *R* subfolder, open *input-slider.R*.
Notice lines **220-231**:

```R
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

* To choose a Ion.Rangeslider theme, you may download the full js library from the
official [website](http://ionden.com/a/plugins/ion.rangeSlider/en.html). 
Extract the corresponding archive and open it. In the *css* folder, you should see 5 different css
skins.

* Go back to the *input-slider.R* file of your shiny source, and replace `css/ion.rangeSlider.skinShiny.css`
by `ion.rangeSlider.skinFlat.css` (You are free to select another one)

* You need now to make an archive of the modified shiny folder (containing the custom
slider skin). Open a **terminal** and go to the directory of your new shiny folder:
```R
$ tar -zcvf shiny_1.0.5.tar.gz shiny_custom
```

* Delete the official shiny package folder (it is stored in the library folder of R). 
On Mac, it is in:

```R
$ cd /Library/Frameworks/R.framework/Versions/3.3/Resources/library/
```

* Open a terminal and go back in the directory containing your own modified shiny package. Then run:

```R
$ R CMD INSTALL shiny_1.0.5.tar.gz
```

This will install your updates. If everything is fine, your sliders should
be different from that used by default in shiny. Be careful that each future update of shiny
will **erase** your modifications. Up to you to **save** this work.