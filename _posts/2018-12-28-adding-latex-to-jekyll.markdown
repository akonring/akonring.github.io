---
layout: post
title:  "Blogging with Latex"
date:   2018-12-28 16:28:55 +0100
author: Anders Konring
toc: true
categories: latex research
tags: [latex, research]
---

If you have ever used [Medium](https://medium.com/) for creating Math and Computer Science related content, you know that it has its limits.
One of these is the ability to support [LaTeX](https://en.wikipedia.org/wiki/LaTeX).

This blog post will show you how to incorporate LaTeX formatting into your Jekyll blog. 

# Ingredients

## MathJax

[MathJax](https://www.mathjax.org/) is a JavaScript display engine for mathematics that uses css and svg to support LaTeX instead of flash or bitmap.

## Kramdown

While Kramdown processes `\( ... \)` and `\[ ... \]` one can use `$$` to indicate that the content is for MathJax to parse[^1].

# Mixing

## Jekyll

If you have a Jekyll engine you can overwrite the existing default.html by adding the file in the `_layouts` directory in the root.

```
_layouts
└── default.html
```

In this case I am using [minimal](https://github.com/pages-themes/minimal) theme and adding the piece of javascript below to the header will ensure that MathJax will parse the page.

```
  <head>
     <script type="text/javascript" async
	    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML">
    </script>
  [...]
```  

# Result

Now, the blog can do blocks of latex:
```
\sum_{k=0}{k^2}
```

$$
\sum_{k=0}{k^2}
$$

As well as inline ($$E=\frac{mc^2}{\sqrt{1-\frac{v^2}{c^2}}}$$) latex.


[^1]: [Test case](https://github.com/gettalong/kramdown/blob/master/test/testcases/span/math/mathjaxnode.text) for Kramdown and Mathjax. 
