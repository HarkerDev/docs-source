---
title: "Front End Dev Practices"
date: 2018-09-23
---

## Placement and External Referencing

Front-end development takes place *solely* in the views and static folders.

### Views

In `views`, there should be two subfolders: `pages` and `partials`. `pages` should only store `.ejs` files that are not going to be included in other pages. `partials` should only store `.ejs` files that are going to be reused in files in the `pages` folder.

### Static

In `static`, there may be many folders, including `css`, `js`, `img`, `fonts`, and so on. Any non-ejs file that is to be referenced in the ejs files. As should be obvious, stylesheets belong in `css`, javascript scripts belong in `js`, images belong in `img`, and fonts belong in `fonts`. Any static file should be stored in `static`. A purely html file that is not dynamic could potentially be stored in `static`.

Any stylesheet or script that will be used multiple times should be stored in these folders. Otherwise, it can be stored within the ejs file itself.

Since static should be hosted at `/`, to access a static file, the `src` or `href` should be `/` + the relative location of the file to the `static` folder.

### References

#### JavaScript

Scripts must have a certain order in order to function properly. For example, any javascript that requires `jquery` must be run after the `jquery` script has finished. This can be accomplished be either placing the script after the `jquery` script or by adding a `defer` attribute.

Scripts that are used multiple times (i.e. external scripts) generally should be placed in the `<head>`, accounting for the order that allows for the scripts to run properly (e.g. placing jquery before Materialize, and then a script that references Materialize after the Materialize script). Scripts that are defined within the file should be placed in the `<body>`.

#### CSS

Generally, all styling should either be placed in an external stylesheet or within a `<style>` tag in the `<head>`. CSS that is specific to that page only should be placed in a `<style>`, while CSS that may be used in other pages as well should be placed in an external stylesheet (in `static`). `style` attributes should not be used; instead, those elements should be given either an `id` or `class` which has style defined either in an external stylesheet or `<style>` tag.

### EJS Includes

At HarkerDev, we only use `include` to include files from `views/partials`. If you are going to use a medium-to-large sized chunk of HTML multiple times, make a custom partial and `include` it. If the chunk of HTML is a `<style>` or `<script>` tag, do not use includes, but rather an external stylesheet or script.

## Commenting

### HTML

In large chunks of HTML, have a starting tag describing the chunk, and then another comment closing the chunk with the selector, like so:

{{< highlight html >}}
<!-- This does something -->
<div class=".selector">
  <!-- This does another thing -->
  <div class=".something-else">
    <!-- A lotttt of code -->
    ...
  </div><!-- /.something-else -->
</div><!-- /.selector -->
{{< / highlight >}}

### JavaScript

When defining variables, please try to make the variable name self-explaining. In addition, leave a comment next to it briefly describing its purpose like so:

{{< highlight js >}}
var interval = 50        // the interval for the setInterval() function used by the countdown timer
var isDisplaying = false // whether the page is currently displaying
{{< / highlight >}}

As shown above, if variables are set in blocks, align the comments for a nicer aesthetic. If your comment uses the word 'whether' for a boolean, make sure that the comment aligns with your boolean; if the condition set after the word 'whether' is true, your variable should be equal to `true`. Never say 'whether or not'.

Before functions, add a comment describing the purpose and/or the action of the function right before it. If it is long, add some comments describing different parts of the function:

{{< highlight js >}}
/*
  does something

  param   - whether to do something
  another - number describing something else

  returns a float which means something
*/
function something(param, another) {
  // first does a thing
  // then another thing
  // finally does this
  return x
}
{{< / highlight >}}

If you got the code off of another website, make sure to link them in the comment:

{{< highlight js >}}
/*
 https://stackoverflow.com/questions/123456/does-this-really-cool-thing
 does a really cool thing
*/
function something() {
  // first does a thing
  // then another thing
  // finally does this
}
{{< / highlight >}}

Also, try to comment before calling functions with callback, like so:

{{< highlight js >}}
// after doing x, does y
funcWithCB(function() {
  // first does a thing
  // then another thing
  // finally does this
})
{{< / highlight >}}

Unless it is obvious, like the `jquery's $(document).ready(callback)` function, which can abbreviated to `$(callback)`:

{{< highlight js >}}
$(function() {
  // first does a thing
  // then another thing
  // finally does this
})
{{< / highlight >}}

Long explanations are not needed, and functions should be generally self-descriptive. Function names should not be more than 15 characters, and abbreviations can be used as long as comments are added with the full name.

Put a single blank line in between blocks of code for easier reading.

### CSS

Generally, comments should not be used except to separate CSS into different sections. Otherwise, selectors should be completely self descriptive, while using abbreviations (such as `btn` instead of `button`). Selectors use hyphens (`-`) to separate different words (e.g. `sm-btn` for `small button`).

## Indentation

If using Atom as the editor, the Auto Indent feature should handle everything. If a line is too wide (longer than the width of the screen, or 100 characters), then it should be split into multiple lines.

## AJAX vs. EJS printing

EJS allows developers to display information on the front-end from the back-end without using AJAX. You can access other partials by using `include`. You can insert any information from `res.locals` or `app.locals` by using the `<%-` and `<%=` tags. For more information on how to use ejs, [view their tutorial](http://ejs.co/).

Whether to use an AJAX request or use EJS to display information is a decision to be made by the team working on that part of the site (including backend and frontend). Using an AJAX request allows for a more developed REST API, but using EJS is more simple. If a request for information is commonly used throughout pages, making an API route may be the better option. However, if data is specific to the page, and does not change often, using EJS may be better. That being said, the decision requires a strong communication between the back-end and front-end developers.

An example of a good time to use EJS is when generating a PDF, or a page designed to be printed. That way, the page is ready to be printed immediately after rendering, rather than having to wait for an AJAX request to finish.

An example of a good time to use AJAX is when data is often refreshed, such as a user's data, or if that data needs to be modified.

<br><br>