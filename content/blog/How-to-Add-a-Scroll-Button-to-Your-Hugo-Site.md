+++
title = "How to Add a Scroll Button to Your Hugo Site"
date = 2024-10-31
description = ""
tags = [
    "Hugo",
    "Custom",
]
categories = [
    "Development",
    "Hugo",
]
series = ["Hugo"]
author = "Li Xiang"
show_summary = false
toc = true
+++

In this article, I will show how to add a simple Scroll-to-Top button to your webpage.

Vanilla Back To Top is simple and tiny with no dependencies. Hides when on top, scrolls up smoothly when clicked. Works equally great with Vue, React, Angular and without frameworks on Jekyll, Hugo and Hexo

What we need to do first is go to [vfeskov](https://github.com/vfeskov)'s [vanilla back to top repo](https://github.com/vfeskov/vanilla-back-to-top), there provides 3 ways to use it.

## Use the file provide on unpkg.com
You can find [How to use](https://github.com/vfeskov/vanilla-back-to-top#how-to-use) if you read the README.

Just add below to your Single page templates, for example `../layouts/post/single.html`.

```html
<script src="https://unpkg.com/vanilla-back-to-top@7.2.1/dist/vanilla-back-to-top.min.js"></script>
<script>addBackToTop({
  diameter: 56,
  backgroundColor: 'rgb(255, 82, 82)',
  textColor: '#fff'
})</script>
```

## Use it locally
You know, sometimes when network is not stable, your button may not show because it rely on remote, so I would recommend you to download it locally to keep your website render normally **(NOTE: Only if your website is not too big, or using cdn is a good way.)**

So just go to the [page](https://unpkg.com/vanilla-back-to-top@7.2.1/dist/vanilla-back-to-top.min.js), copy the code, name it as `vanilla-back-to-top.min.js` and save it in your custom js folder, for example `../static/js/vanilla-back-to-top.min.js`.

Then just like before, add below code in your single page templates `../layouts/post/single.html`.

```html
<script src="/js/vanilla-back-to-top.min.js"></script>
<script>addBackToTop()</script>
```

## With npm
You can also install the package via npm and import it into your own bundle. Run below code in your hugo project's root folder.

```js
npm install --save vanilla-back-to-top
```

Then import it as ES6, Node.js or RequireJS module, for example:

```js
import { addBackToTop } from 'vanilla-back-to-top'
addBackToTop()
// your code here
```

## Custom

The default params are shown below, you can always custom it yourself.

```html
addBackToTop({
  backgroundColor: '#000',
  cornerOffset: 20, // px
  diameter: 56, // px
  ease: inOutSine, // any one from https://www.npmjs.com/package/ease-component will do
  id: 'back-to-top',
  innerHTML: '<svg viewBox="0 0 24 24"><path d="M7.41 15.41L12 10.83l4.59 4.58L18 14l-6-6-6 6z"></path></svg>',
  onClickScrollTo: 0, // px
  scrollContainer: document.body, // or a DOM element, e.g., document.getElementById('content')
  scrollDuration: 100, // ms
  showWhenScrollTopIs: 1, // px
  size: diameter, // alias for diameter
  textColor: '#fff',
  zIndex: 1
})
```

|Option	|Description|
|:------:|:-----:|
|id|	id attribute of the button|
|showWhenScrollTopIs	|Show the button when page got scrolled by this number of pixels|
|onClickScrollTo|	Where to scroll to, in pixels, when the button gets clicked, 0 means the very top|
|scrollDuration|	How long, in milliseconds, scrolling to top should take. Set to 0 to make scrolling instant|
|innerElement|	DOM element to put inside the button; with jQuery you can put something like this: $('\<svg>...\</svg>').get(0)|
|size|	Width and height of the button in pixels
|cornerOffset	|Right and bottom offset of the button relative to viewport|
|backgroundColor|	Background color of the button|
|textColor	|Text color of the button|
|zIndex|	z-index of the button|
|scrollContainer	|If only part of your website gets scrolled, e.g., when your sidebar never scrolls with content, put the scrolled DOM element here|

And here is my config, you can see what it looks like at the right-down corner.

```html
<script src="/js/vanilla-back-to-top.min.js"></script>
<script>addBackToTop({
  backgroundColor: '#fff',
//  cornerOffset: 20, // px
  diameter: 45, // px
//  ease: inOutSine, // any one from https://www.npmjs.com/package/ease-component will do
//  id: 'back-to-top',
  innerHTML: '<svg viewBox="0 0 24 24"><path d="M4 12l1.41 1.41L11 7.83V20h2V7.83l5.58 5.59L20 12l-8-8-8 8z"/></svg>',
//  onClickScrollTo: 0, // px
//  scrollContainer: document.body, // or a DOM element, e.g., document.getElementById('content')
  scrollDuration: 777, // ms
//  showWhenScrollTopIs: 1, // px
//  size: diameter, // alias for diameter
  textColor: '#000',
//  zIndex: 1  */
  })</script>
```