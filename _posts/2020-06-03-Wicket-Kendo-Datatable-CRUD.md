---
title: Wicket Kendo Datatable, aw, problem solved. :)
author: Shilan Kalhor
date: 2020-06-03 10:59:00 +0800
categories: [Coding, Springboot, Wicket, KendoUI, Datatable, Grid, CRUD]
tags: [java8, Wicket, KendoUI, Datatable]
toc: false
---

I had to and I emphesize on this that I had to work on a Wicket 8 project which needed a grid with inline editing.
Well in the beginning it sounds easy. Every single UI framework will provide you with tons of samples on how to implement grids and add
CRUD functionalities to, *BUT NOT* Wicket!

I had a hard time finding an standard native solution, impossible! Library or something that does support inline editing with Wicket when I finally came across Kendo UI library.
The [Wicket KendoUI Document](http://www.7thweb.net/wicket-jquery-ui/kendo/datatable/CommandsDataTablePage) looked shiny and promising but then later came problems.

Adding the dependencies and everything you expect that your simple datatable should work but it doesn't.

There is this small sign right in front of Datatable menu item --> (Pro)
But what does that mean?! That's what I missed in first place.
It means there is a javascript file called `kendo.min.all.js` that doesn't come along with the component. But **why?!** Because this file and
specifically this file has a different type of license from the rest of KendoUI javascript files and it is only allowed for Kendo Pro (Paid) users!
So if you want to implement KendoUI datatable you need to either download this file or use the CDN link of it!
Well I am working in a company that direct hotlinks are not allowed so I had to suffer even more.

Fortunately the author of Kendo datatable [Sebastian](https://github.com/sebfz1/wicket-jquery-ui/issues/318)
was very kind and helpful to guide me how to fix this issue and actually there I realized that I cannot
use this component due to license violation unless I buy it!

That was the end of story under my situation. I moved to another native, ugly, modal solution that took me ages to build and that's the code 
we have in production now.

However I thought I can at least share the implementation with you, so that if you don't have the license issue, you would be able to easily use this
component and enjoy the elegant interface of it.

## Jump to code!
Here is the github link:
[https://github.com/shilan/wicket-kendo-datatable](https://github.com/shilan/wicket-kendo-datatable)

Another thing that I was missing from KendUI datatable documentation was all sort of things and tricks that you can do with the the datatable columns.
Like making one column disable, invisible or return select box combo editor.
In this code I also tried to cover those to some extend.

The pagination setting parameters still comes as mystery to me, there is no good documentation on how to customize it to show e.g. 10 items
on each page regardless of number of items. And why it keeps showing the pagination bar despite of having one item in the grid.
If you have any solution for that please ping me. I can add it to this code.

Last but not least, **If you you cannot use KendoUI**, and you don't know how to implement modal based CRUD on datatables with native Wicket, let me know, 
I can help you with that. I have already implemented that and it kind of works. :)
Who knows I may add another post for that later. Right now I am just happy that I don't need to work with this beast for a while.




