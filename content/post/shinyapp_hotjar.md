+++
  title = "Track a website usage with hotjar"
  date = "2018-04-01"
+++

A web tracking tool I often use is [hotjar](https://www.hotjar.com). Below is a heatmap
showing all the users click in the past week in my personal website. I am keen on
this kind of feedback since it summarizes everything at a glance. The screenshot
you can see only takes into account click events. However, you can also see users
moves or how they scroll on your website.

<a href="/images/heatmap_blog.jpg"><img src="/images/heatmap_blog.jpg" width="auto" height="auto" alt="blank"></a>

The table below shows where your visitors are from, how much time they spend on a
given page, with which web browser and when. 

<a href="/images/visitors_logs.png"><img src="/images/visitors_logs.png" width="auto" height="auto" alt="blank"></a>

Moreover, you can go further and investigate what they specifically did during their
session. Indeed, each user session is recorded so that you see exactly where they spent
a lot of time, you can even detect hesitations and at what moment they exited your
website.

<a href="/images/visitors_movie.png"><img src="/images/visitors_movie.png" width="auto" height="auto" alt="blank"></a>

If you are interested in the features described above, I highly recommend you
trying hotjar and create a free account (perfect for small impact websites). Furthermore,
if you expect your personal website to have a lot of trafic, you might consider
upgrading your account. To configure hotjar, everything is really well described 
in their help section [here](https://help.hotjar.com/hc).

Basically, hotjar only needs a tracking code to work properly. Be sure to provide
the good url and simply check your installation with their tool.

{{< highlight html>}}
<!-- Hotjar Tracking Code for https://divadnojnarg.github.io -->
        <script>
            (function(h,o,t,j,a,r){
                h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
                h._hjSettings={hjid:827666,hjsv:6};
                a=o.getElementsByTagName('head')[0];
                r=o.createElement('script');r.async=1;
                r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
                a.appendChild(r);
            })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
        </script>
{{< /highlight >}}

<a href="/images/hotjar_check.png"><img src="/images/hotjar_check.png" width="auto" height="auto" alt="blank"></a>

You can easily understand that hotjar can be used for tracking a shiny app usage.
To include it in your app, you have 6 steps to follow:

- create a new url tracking in the hotjar dashboard and provide the 
url of your shinyapp
- a tracking javascript code (as above) will be given to you
- copy the part embeded in the script tag and create a new file called hotjar.js
in the **www folder** of your shiny app

{{< highlight javascript>}}
javascript
(function(h,o,t,j,a,r){
                h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
                h._hjSettings={hjid:827666,hjsv:6};
                a=o.getElementsByTagName('head')[0];
                r=o.createElement('script');r.async=1;
                r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
                a.appendChild(r);
            })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
{{< /highlight >}}

- in the **ui.R** file, insert the following code:

{{< highlight r>}}
tags$head(includeScript(paste0(getwd(), "/www/hotjar.js")))
{{< /highlight >}}

- Check if your tracking code is active as shown above. When a green message appear,
hotjar is succesfully installed.
- Do not forget to set up a heatmap as well as recording (left sidebar of the hotjar
dashboard). The configuration is pretty straightforward that is why I will not describe it here.

Finally, I give you an overview of what you could obtain with a shiny app. I noticed however
 that I was not able to see any input object in the hotjar recorded movie. I have no idea of what is
 missing to make it perfect. Consequently, any feedback is welcome!
 
 Here is the Hotjar feedback regarding this issue:
 
 > Hi David,

>Thanks for reaching out and I'm so sorry about this trouble!

>I had a look at your heatmap and I believe the reason this may be happening is due to the fact that these elements are tied to canvas objects. Currently, Hotjar doesn't support interactions with SVG or Canvas objects, so they would appear in your heatmap and thus your knobInput examples return a void result.

>That said, we already have this in our development pipeline. The more times an item is mentioned by our users the higher we prioritize it and the faster we will get it done - so this just got upvoted. 😃

>I have taken a note to inform you as soon as this becomes available.

>In the meantime, you can see what we're currently working on in our Product Roadmap.

>Hope that cleared things up and please let me know if you need any extra help!
 
 
 <a href="/images/heatmap_shinyapp.jpg"><img src="/images/heatmap_shinyapp.jpg" width="auto" height="auto" alt="blank"></a>
