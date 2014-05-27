---
title: GridList - building responsive dashboards with resizable widgets
author: Ovidiu Chereches
layout: post
categories:
  - Our projects
  - Open source
---

##What?

[GridList](1) is a js library for creating two-dimensional, resizable and responsive list of items that we built to help create highly customizable dashboards. The library is used in one of our flagship features we call [Boards](2) and was developed with the needs of this project in mind.

##Why a grid?

Our strongest point is our data, this is what we do best, collect and analyze great volumes of data. Data alone is not enough. It is critical to show the result of all the effort of collecting and analyzing in an easy to comprehend way. We needed a way to make it easy to discover patterns and take action on them.

We needed a data visualization tool that would be:

 -  easy and intuitive to use;
 -  easy to customize and manage;
 -  responsive - meaning that it will look great no matter what screen it is shown on, a mobile phone or a big 4k display;

A dashboard is a way of visualizing data that, if properly built, would tick all the boxes of our requirements. We are passionate about dashboards and went through a few iterations and versions of displaying data in a dashboard form to understand what makes a dashboard great.

Given all of these it was clear that we needed to use a grid system that supported drag and drop. We had two options, find and adapt an existing one or make our own.

##Why build a new grid system?

In the end we decided to build our own library. We looked at existing grid systems and most closely at gridster.js, but all of them fell short and did not satisfy our requirements fully or would have required too much effort to adapt.

Our requirements for the grid system were:

 -  support of horizontal grids - gridster.js works vertically and our design is horizontal. We looked at making  gridster.js work both vertically and horizontally but the code required too much effort and was too complex to approach.
<img style="width:100%" alt="Mozaic logo" src="{{ site.url }}/images/gridding/1_vertical.gif" />

 -  responsive - as mentioned earlier, it had to work on all kind of displays
 -  needed full height widgets for our timelines, a particular type of widget that occupies a full column in horizontal grids or a full row in vertical ones, a feature that gridster.js did not have
 -  we wanted the grid logic to be a DOM-less library outside the jQuery plugin. This allows us to compute grid positions on the server-side and run extremely fast tests with Node.
 -  very good UX experience that would not frustrate users with large boards in which the order of the items was important. This is a major point which is very difficult to figure out as it is something quite subjective. We consider that we nailed this part by implementing a well thought collision mechanism that is a step ahead of the basic collision mechanism implemented by gridster.js (https://github.com/ducksboard/gridster.js/issues/54) and other similar libraries and by following a few principles that are discussed in depth later.

While solving all of this, our library ended up having 5 times fewer lines of code then gridster.js.


##Principles

When building this project we stuck to a few principles:

**Don’t brake user input**

This is a fundamental part of GridList and one that is easily missed. It is what gives the feeling that it works as it should or as expected when you drag an item around. The principle can be described best as - no surprises for the user. When you drag an item to a new position, that item will be placed there and nowhere else. The grid system will not do any magic afterwards and start moving the item around to make it fit. After an item is placed in the desired position the collision mechanism kicks in and the items that have to move are arranged so that as few changes of position are needed.
<img style="width:100%" alt="Mozaic logo" src="{{ site.url }}/images/gridding/2_collisions.gif" />
Collisions can be solved in two ways. First, an attempt to resolve them locally is made, meaning that the moved item tries to swap position with the overlapped item(s). This is the preferred fair trade. If this doesn't work out and after swapping we still have collisions inside the grid, the entire grid will be recalculated, starting with the moved item fixed in its new position. In the latter case, all the items around and to the right of the moved item might have their position slightly altered.

**As independent from jquery and other libraries as possible.**

This was achieved by splitting the GridList library in two parts, based on their roles:
a) An agnostic GridList class that manages the two-dimensional positions from a list of items within a virtual matrix
b) A jQuery plugin built on top of the GridList class that translates the generic items positions into responsive DOM elements with drag and drop capabilities
Only the second part has anything to do with the actual DOM, the core of the grid system does not need to know about it.
The jQuery part only translates the results of the computations into pixels and handles drag and drop events.

**The size of the grid can be changed at any time**

This makes it possible to have a responsive grid. it also makes it easy to have zoom in and out controls. What we looked for here is to keep the widget positions in place as much as possible on grid size changes.
<img style="width:100%" alt="Mozaic logo" src="{{ site.url }}/images/gridding/3_resize.gif" />

**Agnostic to how the grid is stored and efficient with reads and writes**

We made the grid be completely independent of how the actual grid data is saved.
We also put a lot of effort into making sure we only send the actual changes. Once an item is dragged and dropped into a new position the grid system will only notify about the widgets that have a new position, instead of resending the whole grid
And we went a step even further by supporting bulk requests.

**Built for the opensource community**

The whole project was conceived as being open source from the very start. We are heavy users of many open source projects and tools and contribute to many of them. This was an opportunity in which we had something to give back so we wrote GridList from the start as an open source project and we will continue to improve and maintain it.

##The result

The effort of building the grid materialized in our boards feature. We made a horizontal, responsive, resizable dashboard that showcases the data we crunch very well.
<img style="width:100%" alt="Mozaic logo" src="{{ site.url }}/images/gridding/4_board.gif" />

If you want to start using GridList, find more about the technical aspects or contribute, check out the [GitHub page](1)

 [1]: https://github.com/uberVU/grid
 [2]: http://media.hootsuite.com/ubervu-via-hootsuite-boards/