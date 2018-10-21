---
title: "Referencing a Partial Page"
date: 2018-09-24
---

To reference the partial page `views/partials/some-partial.ejs` from a page in `views/pages` in ejs, use the `include` function as so:

{{< highlight html >}}
<p> Here is some HTML </p>
<%- include('../partials/some-partial.ejs') %>
<p> Here is some HTML </p>
{{< / highlight >}}

This will replace `<%- include('../partials/some-partial.ejs') %>` with whatever code is in `views/partials/some-partial.ejs`.

For more information on working with `ejs`, [click here](http://ejs.co/).

For information on what the various distributed partials do, [[view this tutorial|https://github.com/DJMcoder/HarkerDevGuides/wiki/Using-the-Various-Partials]].