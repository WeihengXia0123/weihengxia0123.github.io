---
title: "MathJax with Hugo and markdown"
date: 2020-12-04T10:12:28+01:00
categories:
- markdown
tags:
- mathJax
- markdown
keywords:
- markdown
#thumbnailImage: //example.com/image.jpg
---

It's actually very easy to add math equations in Markdown, using MathJax. For example:
$$
R = exp(\hat{\phi})
$$


<!--more-->
## How to use MathJax with Hugo and Markdown

Since I'm using Hugo framework to generate this static blog website, I referred to their official site and it says to use MathJax, BUT!!! they didn't tell us where to add this one line of javaScript code!



Then I referred to [butui's blog](https://butui.me/post/yet-best-math-formula-support-for-hugo-with-mathjax/) on math equations with hugo, it's actually very simple. We just need to add the below code into `hugo/themes/[your_theme_name]]/layouts/partials/footer.html`

```
<footer id="footer" class="main-content-wrap">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_SVG"></script>
</footer>
```

Then you can feel free to write anything down like:

```
$$
	R = exp(\hat{\phi})
$$
```

Output:
$$
R = exp(\hat{\phi})
$$

{{< alert warning>}}
Butui's blog also mentioned to use `markup:mmark`, but I didn't use and it works fine so far. So I'm not totally sure if we really need it.
{{</alert>}}
{{< alert success>}}
Actually we NEED `markup:mmark` because it can help us to use more complex mathJax functions like `\begin{aligned}`.
{{</alert>}}

Note: \
[1] Remember to have one extra line between your text and your mathJax code.\
[2] The code must be left aligned! No EMPTY SPACE! NO INDENT! Such as below:
```latex
Balah balah:

$$
\begin{equation}
\begin{aligned}
\dot{R}_{ib} &= \frac{dR_{ib}}{dt}\\
&= \lim_{\triangle{t}\to 0}\frac{R_{ib}exp([w^b\triangle{t}]^{\wedge})-R_{ib}}{\triangle{t}}\\
&= \lim_{\triangle{t}\to 0}\frac{R_{ib}(I+[w^b\triangle{t}]^{\wedge})-R_{ib}}{\triangle{t}}\\
&= \lim_{\triangle{t}\to 0}\frac{R_{ib}[w^b\triangle{t}]^{\wedge}}{\triangle{t}}\\
&= R_{ib}[w^b]^{\wedge}\\
\end{aligned}
\end{equation}
$$
```

Reference: \
[1] [butui blog](https://butui.me/post/yet-best-math-formula-support-for-hugo-with-mathjax/)\
[2] [xlmaverick blog](https://www.xlmaverick.me/post/%E8%A7%86%E8%A7%89%E5%89%8D%E7%AB%AF/)
