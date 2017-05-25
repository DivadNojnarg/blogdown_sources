+++
  title = "Setting MathJax with Hugo"
  date = "2017-05-22"
+++
    
## The classical approach

[MathJax](https://www.mathjax.org) is a javascript library which allows the user to integrate math expressions in a website or a blog. However, its [configuration](http://docs.mathjax.org/en/latest/configuration.html) can be quite complex for beginners. In my previous website powered by Jekyll, I easily managed to install MathJax, whereas I encountered some problems with Hugo. 

### 1. Installing MathJax via Hugo's documentation

Everything you need can be found [here](https://gohugo.io/tutorials/mathjax/). You have to insert the code below in the */layout/partials* folder of your website: it is even better to create a file named **mathjax_support.html**, for example.

```javascript
<script type="text/javascript"
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
```

Then, you will have to include **mathjax_support.html** in **footer.html** or **header.html**. In my case, I chose to put it in **header.html** just before the closing `</head>` as follows:

```
{{ partial "mathjax_support.html" . }}
```

If you are as lucky as me, it should not work at all :). 

### 2. A second method

I found a [solution](https://discuss.gohugo.io/t/solved-mathjax-stopped-working/5946) after some google research and I warmly thanks its author! The idea is to slightly modify the previous code by including everything in the same `<script>` and adding *async*:

```javascript
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
  MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
  });
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
  });

  MathJax.Hub.Config({
  // Autonumbering by mathjax
  TeX: { equationNumbers: { autoNumber: "AMS" } }
  });
</script>
```

Now, your **mathjax_support.html** should look like the above code and you should be able to play with latex!

<span style="color:red">**According to MathJax, their CDN is being [discontinued](https://www.mathjax.org/cdn-shutting-down/), you are not concerned if you use the previous code.**</span>

## Latex rendering errors

There are some differences with classical Latex expressions and the syntax to use in a markdown document. For example, `\sum_` does not render with Hugo and you should use `\sum\_` instead (notice the second backslash before the underscore). I noticed that it is the same thing for `\int_` (so `\int\_`). Of course, this will tighly depend on the configuration you chose for MathJax. Moreover, the following system of equations does not render properly:

$$
\left\{
\begin{align}
\dot{x} & = \sigma(y-x) \newline
\dot{y} & = \rho x - y - xz \newline
\dot{z} & = -\beta z + xy
\end{align}
\right.
$$

What follows is better:

$$
\left\\{
\begin{align}
\dot{x} & = \sigma(y-x) \newline
\dot{y} & = \rho x - y - xz \newline
\dot{z} & = -\beta z + xy
\end{align}
\right.
$$

by simply changing `\left\{` to `\left\\{`. Yet, `{` should be displayed in the center and not in the left, a problem that I did not have with Jekyll. Thus, I replaced `\begin{align}` by `\begin{cases}`:

$$
\begin{cases}
\dot{x} & = \sigma(y-x) \newline
\dot{y} & = \rho x - y - xz \newline
\dot{z} & = -\beta z + xy
\end{cases}
$$

which works, except that we lost equations numbering.

## Latex Showcase

$$
\left( \sum\_{k=1}^n a_k b_k \right)^{2} \leq
 \left( \sum\_{k=1}^n a_k^2 \right) \left( \sum\_{k=1}^n b_k^2 \right)
$$

$$
\mathbf{V}_1 \times \mathbf{V}_2 =
   \begin{vmatrix}
    \mathbf{i} & \mathbf{j} & \mathbf{k} \newline
    \frac{\partial X}{\partial u} & \frac{\partial Y}{\partial u} & 0 \newline
    \frac{\partial X}{\partial v} & \frac{\partial Y}{\partial v} & 0 \newline
   \end{vmatrix}
$$

$$
\frac{1}{(\sqrt{\phi \sqrt{5}}-\phi) e^{\frac25 \pi}} =
     1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {1+\frac{e^{-6\pi}}
      {1+\frac{e^{-8\pi}} {1+\ldots} } } }
$$

$$
1 +  \frac{q^2}{(1-q)}+\frac{q^6}{(1-q)(1-q^2)}+\cdots =
    \prod_{j=0}^{\infty}\frac{1}{(1-q^{5j+2})(1-q^{5j+3})},
     \quad\quad \text{for $|q|<1$}.
$$

$$
\begin{align}
  \nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} & = \frac{4\pi}{c}\vec{\mathbf{j}} \newline
  \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \newline
  \nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \newline
  \nabla \cdot \vec{\mathbf{B}} & = 0
\end{align}
$$

$$
\begin{equation} 
x(t) = e^{\int\_{t_0}^tp(s)ds}\Bigg(\int\_{t_0}^t\Big(q(s)e^{-\int\_{t_0}^sp(\tau)d\tau}\Big)ds + x_0\Bigg).
\end{equation}
$$

Here is inline math: $\sqrt{3x-1}+(1+x)^2$.

**I will update this part as frequently as possible...**