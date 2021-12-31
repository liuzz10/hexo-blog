---
title: How to Add Google Analytics to Hexo Site of Any Theme (2021)
date: 2021-12-30 14:50:17
tags:
  - blogging
  - programming
---

Since Google Analytics(GA) changed the layout, some tutorials are too old to follow. So I'm writing an up-to-date one.

# Prerequisites

You should have installed Hexo and had the site deployed already (so you should have a site url).

Not sure how to use Hexo and deploy to GitHub Pages? Follow this [tutorial](https://gist.github.com/btfak/18938572f5df000ebe06fbd1872e4e39).

# Instructions

## Firstly, let GA know the site url.

1. Create an GA account. Google the instruction and it's pretty easy to follow. Make sure that you entered the site url so that Google Analytics knows what site to track.

2. Go to Admin (bottom left corner) > Data Streams. You should see the site url that you entered. Click on it.

![hexo1](hexo1.jpg)

3. Go to Tagging Instructions > Add new on-page tag > Global site tag (gtag.js).

![hexo2](hexo2.jpg)

4. Copy all the code provided.

![hexo3](hexo3.jpg)

## Secondly, let the site know GA.

1. Go to your Hexo code. Under your template's folder, find `layout.ejs`.

2. Paste the site tag code in `<head></head>` and make sure it's on the top of other code in `<head></head>`.

```HTML
<!DOCTYPE html>
<html lang="<%=config.language%>">
  <head>
    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script
      async
      src="https://www.googletagmanager.com/gtag/js?id=XXXXX"
    ></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag() {
        dataLayer.push(arguments);
      }
      gtag('js', new Date());

      gtag('config', 'XXXXX');
    </script>
    <!-- other stuff in header -->
    ...
  </head>
  <body>...</body>
</html>
```

## Finally...

You can do `hexo clean`, `hexo deploy` to deploy it. It's just this simple! Make sure that you verify it by checking the report dashboard on GA.

**Reference**
https://jiwonyeom.github.io/2017/10/09/google-analytics-on-hexo/
https://www.codeblocq.com/2015/12/Add-Google-Analytics-to-your-hexo-blog/
