---
title: "Add Math Equations in Markdown with one line of code"
date: 2020-12-09T10:12:28+01:00
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

