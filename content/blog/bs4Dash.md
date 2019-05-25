+++
  title = "Build Bootstrap 4 dashboards with bs4Dash"
  date = "2018-07-05"
+++

<style type='text/css'>
.card {
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    border-radius: 5px; /* 5px rounded corners */
}

.card:hover {
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

/* Add rounded corners to the top left and the top right corner of the image */
img {
    border-radius: 5px 5px 5px 5px;
}

/* Add some padding inside the card container */
.container {
    padding: 2px 16px;
}
</style>

## Introduction <img src="/images/bs4Dash1.png" width=200 align="right" />

**shinydashboard** is currently built on top of [AdminLT2](https://adminlte.io/themes/AdminLTE/index2.html) and uses bootstrap 3. 
Yet, bootstrap 4 is already released and nothing was available for shiny, regarding
dashboards. bs4Dash is built on top of [AdminLTE3](https://github.com/almasaeed2010/AdminLTE/tree/v3-dev) and
bring also extra components from bootstrap 4. 
The syntax is very close to that of shinydashboard so that users are not lost. 
Find out more on [github](https://github.com/DivadNojnarg/bs4Dash).

## Installation

You can install this package from CRAN or the latest dev version via github:

```r
# from CRAN
install.packages("bs4Dash")
# devel version
devtools::install_github("DivadNojnarg/bs4Dash")
```

## Showcase

<div class="row">
<div class="col-sm-4" align="center">
<div class="card">
<a href="https://rinterface.com/shiny/showcase/lorenz/" target="_blank"><img src="/images/bs4Dash_Lorenz.png"></a>
</div>
</div>
<div class="col-sm-4" align="center">
<div class="card">
<a href="https://rinterface.com/shiny/showcase/lotkaVolterra/" target="_blank"><img src="/images/bs4Dash_Lotka.png"></a>
</div>
</div>
<div class="col-sm-4" align="center">
<div class="card">
<a href="https://rinterface.com/shiny/showcase/ratp" target="_blank"><img src="/images/bs4Dash_ratp.png"></a>
</div>
</div>
</div>

<br>
