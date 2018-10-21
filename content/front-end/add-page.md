---
title: "Adding a New Page to a Project"
date: 2018-09-24
---

This tutorial will guide you on how to add a new page to a node.js website.

## Creating the page

### Adding the head

First, add the following code to the top of your page:

{{< highlight html >}}
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <%- include('../partials/head.ejs') %>
    <title> Title </title>
  </head>
  <body>
{{< / highlight >}}

If you want to add in a stylesheet, favicon, or `<meta>` tag, you would put it here in between the `<head></head>` tags.

### Referencing Partials

For information on how to reference a partial, [[view this tutorial|https://github.com/DJMcoder/HarkerDevGuides/wiki/Referencing-a-Partial-Page]].

For information on what the various partials do, [[view this tutorial|https://github.com/DJMcoder/HarkerDevGuides/wiki/Using-the-Various-Partials]].

There are three main partial pages to include. The first is the navbar, which usually is the first thing after the `<body>` tag. You would include it as so: 

{{< highlight html >}}
<%- include('../partials/navbar.ejs') %>
{{< / highlight >}}

Then, there is the loader partial, which includes tools on making a full-screen page loader using [[materializecss' preloader tool|http://materializecss.com/preloader.html]]. If you want to use the tools, include them as so:

{{< highlight html >}}
<%- include('../partials/loader.ejs') %>
{{< / highlight >}}

Finally, there is the orientation partial. This partial will force any mobile user to tilt their screen horizontally. To use it, include it as so:

{{< highlight html >}}
<%- include('../partials/orientation.ejs') %>
{{< / highlight >}}

### Adding your own code

Add whatever page content that the page is intended to have. Note that the head include give you access to materializecss and jquery. 

For information on how to use materializecss, [[view their website|http://materializecss.com/]]. 

For information on how to use jquery, [[view their documentation|http://api.jquery.com/]].

In terms of code structure, [[view our tutorial on front-end coding structure and style|https://github.com/DJMcoder/HarkerDevGuides/wiki/Front-End-Coding-Practices]].

### Closing the page

Close the `<body>` and `<html>` tags as so:

{{< highlight html >}}
  </body>
</html>
{{< / highlight >}}

## Adding your page to the navbar

In `views/partials/navbar.ejs`, in both the `ul.side-nav` and the main `<ul>`, add a `<li>` with an `<a>` inside it, linking to the page you just made, as so:

{{< highlight html >}}
<nav>
  <div class="nav-wrapper">
      <a href="/" class="brand-logo">HarkerDev</a>
      <a href="#" data-activates="mobile-demo" class="button-collapse"><i class="material-icons">menu</i></a>
      <ul class="right hide-on-med-and-down">
        <li><a href="/page">Page</a></li>
      </ul>
      <ul class="side-nav" id="mobile-demo">
        <li><a href="/page">Page</a></li>
      </ul>
    </div>
</nav>
{{< / highlight >}}

For more information on the navbar, [[view the materializecss tutorial on navbars|http://materializecss.com/navbar.html]].

## Adding the page to the backend

To add the page to the backend, add the following code to the main router:

{{< highlight js >}}
router.get('/page', (req, res) => { res.render('pages/page.ejs') })
{{< / highlight >}}

<br><br>