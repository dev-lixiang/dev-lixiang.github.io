+++
title = "How to Integrate Comment in Your Hugo Site"
description = ""
tags = [
    "Hugo",
    "Giscus",
    "Comment",
    "Disqus",
]
date = "2024-10-29"
categories = [
    "Development",
    "Hugo",
]
series = ["Hugo"]
author = "Li Xiang"
+++

Someday you suddenly think it is so alone in your blog and you want others can comment on your blogs, so you opened 
[Hugo's Quick Reference](https://gohugo.io/content-management/comments/), and you find this:
> Hugo ships with an internal Disqus template, but this isn't the only commenting system that will work with your new Hugo 
website.

But when you search with Google, DuckDuckGo, or any engine else, you may find most people would suggest you use 
giscus but Disqus for your blog's comments, so you may wanna know the differences between them. But wait, this is not
what this article's aim is. In this article, I will show you how to integrate a comment module but not compare them, only 
experience it yourself, can you find which is more suit for you.

Note that I prefer to use [Hugo partial template](https://gohugo.io/templates/partial/) since that can keep your file structure clean, so I will use the template in the tutorial.

## Disqus
First, let's look at the [Disqus](https://disqus.com/). 

Actually hugo comes with all the code you need to load Disqus. Before adding Disqus to your site, you'll need to 
[set up an account](https://disqus.com/profile/signup/). Then go to *AddDisqusToSite* and click Get Started.

Note that you should remember the **SHORTNAME** you input since this shortname will appear later on as a variable in your 
*config.yml* or *config.toml*.

Keep moving and fill in the form details for your site, after finishing you will obtain a template like this, create a file named `disqus.html` in `/layouts/partials` and paste it.
```html
<div id="disqus_thread"></div>
<script>
    /**
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
    /*
    var disqus_config = function () {
    this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://YOUR-SHORT-NAME.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```

Then add the partial in the footer of the post template, for me it is in `/layouts/posts/single.html`.

```html
{{ .Content }}

<footer>
    {{ partial "disqus.html" . }}
</footer>

{{ end }}
```

Here we come to the last step, add below code inside your *config.yml* or *config.toml*.

```yml
services:
  disqus:
    shortname: your-disqus-shortname
```

Congrats, now it is all finished. :blush:

## Giscus
First of all, we need to go to [Giscus](https://giscus.app/) to make your own configuration. When you finished, you will get a code like this, just save it so we can use later.

```html
<script src="https://giscus.app/client.js"
        data-repo="[ENTER REPO HERE]"
        data-repo-id="[ENTER REPO ID HERE]"
        data-category="[ENTER CATEGORY NAME HERE]"
        data-category-id="[ENTER CATEGORY ID HERE]"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="en"
        crossorigin="anonymous"
        async>
</script>
```

And then, we need to create a partial template named `giscus.html` in `../layout/partials/`, and paste the code below into the file.

```html
{{- if isset .Site.Params "giscus" -}}
  {{- if and (isset .Site.Params.giscus "repo") (not (eq .Site.Params.giscus.repo "" )) (eq (.Params.disable_comments | default false) false) -}}
  <script src="https://giscus.app/client.js"
    data-repo="{{ .Site.Params.giscus.repo }}"
    data-repo-id="{{ .Site.Params.giscus.repoID }}"
    data-category="{{ .Site.Params.giscus.category }}"
    data-category-id="{{ .Site.Params.giscus.categoryID }}"
    data-mapping="{{ default "pathname" .Site.Params.giscus.mapping }}"
    data-reactions-enabled="{{ default "1" .Site.Params.giscus.reactionsEnabled }}"
    data-emit-metadata="{{ default "0" .Site.Params.giscus.emitMetadata }}"
    data-input-position="{{ default "bottom" .Site.Params.giscus.inputPosition }}"
    data-theme="{{ default "light" .Site.Params.giscus.theme }}"
    data-lang="{{ default "en" .Site.Params.giscus.lang }}"
    data-loading="{{ default "lazy" .Site.Params.giscus.loading }}"
    crossorigin="anonymous"
    async>
  </script>
  {{- end -}}
{{- end -}}
```

Same with before, add the partial in the footer of the post template in `/layouts/posts/single.html`.

```html

{{ .content }}

<footer>
    {{ partial "posts/giscus.html" . }}
</footer>

{{ end }}
```

Finally, we can configure the params in the `config.yml` or `config.toml` as below, just input the params we got in the first step, and enjoy. :smile:

```yml
  giscus:
    repo: "YOUR REPO"
    repoID: "YOUR REPOID"
    category: "CATEGORY"
    categoryID: "CATEGORYID"
    mapping: "pathname"
    reactionsEnabled: "1"
    emitMetadata: "0"
    inputPosition: "bottom"
    theme: "preferred_color_scheme"
    lang: "en"
    loading: "lazy"
    crossorigin: "anonymous"
```