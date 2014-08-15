---
title: Github-changelog - robust notification system built on top of github
author: Valentin Radulescu
layout: post
categories:
  - Our projects
  - Open source
---

##What is it?

[Github-changelog][1] provides a mechanism to communicate to the users of your web app that updates are available and that they should reload to see the changes.
**Github repository**: Check it out [here][1]
**Demo**: See [the demo](http://ubervu.github.io/github-changelog)


##The source of the need
As developers, most of us strive to reach the Holy Grail of continuous deployment, so that any member of the team can push fixes and features as soon as these are built. Mean and lean. For most of us gone are the days of huge releases, replaced by an unpredictable and continuous flow of fixes and features. And these can be many. Etsy, a company we admire for their well oiled deployment system, managed to have [517 individuall](http://codeascraft.com/2011/02/04/how-does-etsy-manage-development-and-operations/) deploys in a single month. Although we are not quite at that scale yet, we manage to have 15+ daily deploys on a productive day.

##More about our deploy system
To fully understand our deploy flow you also need to know a few details about our deploy system. Our deploy system hooked to GitHub commits. Every time we update our master repository, the deploy system pushes the latest version to the production servers and sends out a notification for the users to reload the page. Besides this, each pull-request for our main branch also has an attached issue that is closed when the pull-request is merged.

##The challenge
Given that we are always pushing new things to both our backend and our web app, from time to time we need to tell our users to refresh their web app so that they will run the latest version. The refresh is needed because:

-  The new version of the backend code only works with the new version of the web app and until a refresh is done our users might experience problems or errors. Although this is not the most common scenario it is the worst disruption that the deploy of a (correctly working) new version can cause.
-  Users will not see the new features or fixes.

##Solutions
We have given this problem a lot of thought in the last couple of years since we switched to running our one-page app powered by [Mozaic](https://github.com/ubervu/mozaic) as the frontend for our system. As we see it there are 2 ways of notifying users:

-  Notify and force users to do a refresh, whenever a major change arrives in production
-  Notify and let users refresh at their own convenience

For many months we went with the first version. Although this is a fail proof way of making sure users have the latest version it is also fail proof that it will create frustration as you are interrupting someones work flow at random intervals.

We also removed any sort of notification for a few weeks but that also is not ideal. So we went with a solution that does not create frustration and it also notifies users of changes.

##Enter github-changelog
This is where [github-changelog][1] comes in. By using this simple jQuery plugin, every time an issue, marked as important through a label, is closed in your GitHub repository, the users will get notified.
<img style="width:100%" alt="Mozaic logo" src="{{ site.url }}/images/github-changelog/1_changelog.gif" />

**What problems it solves**:

- It gives a non-intrusive but still highly visible way of communicating when it a change is deployed.
- It offers an automatically generated changelog

As this way of working is not particular to us, we decided to create a plugin that would be easy to integrate in other web applications.

##How we built it
We've always placed great importance in understanding a problem before working on it. The best solution is the one that requires the least amount of work to achieve the expected result.
The first step taken was to understand the need that was going to be addressed: our users needed a better, less intrusive, notification system.

Showing a pop-up and forcing users to reload the app every time an important change would reach production was not cutting it any more.

We could have gone a multitude of ways in how to make this notification system look and behave and there was much discussion around this. Some of the ideas were:

- Facebook style notifications where a notifications icon is visible at all times but that didn't seem right. Facebook shows a very high number of updates.
<img style="display:block; margin: 0 auto;" alt="Mozaic logo" src="{{ site.url }}/images/github-changelog/2_facebook_notification.png" />
- Only show an updates button or section when there are updates. What we were showing to the user were in fact app updates and we wanted to show them in a manner similar to how traditional software updates are shown in desktop apps, in particular the GitHub for Mac app. This piece of software gave us much of the inspiration for the final solution.
<img style="display:block; margin: 0 auto;" alt="Mozaic logo" src="{{ site.url }}/images/github-changelog/3_github_notification.png" />


Since our app was already using GitHub commit hooks to send these messages, we decided to build on that and use the GitHub API rather than making a completely separate notification system. This made sense because this way the process could be automated. Each new feature we ship is tied into a GitHub issue that gets closed. We could in turn show information from that issue to the user so that he can understand what changes are deployed.

What followed were a series of discussions on how to tackle the issue. We talked with our engineers and UI guys, took notes and wrote documentation. Before writing any code we needed to know how the feature we were building would look and how it would work.
From these talks we came up with the basic HTML structure that would be used by plugin. We built wireframes, discussed usage scenarios and then came up with a plan on how to style the HTML.
<img style="display:block; margin: 0 auto;" alt="Mozaic logo" src="{{ site.url }}/images/github-changelog/4_chagelog_wireframe.png" />

The plugin needed only basic styling, as anyone implementing github-changelog is likely to write his own styles that match his app's design.

The things that we focused on here were:

- positioning - there should be a way to position the list in any corner of the screen and still have it be visible
- maintenance - we chose to use LESS to write the styling because it makes maintaining CSS easier and cleaner.

The last and most important step was to determine how the plugin would actually work. We came up with a list of options that the plugin could accept and a list of methods that it would have.

What followed was a small [demo](http://codepen.io/mihneadb/pen/yhrHi?editors=001) to test that we could actually get the data from GitHub which in turn grew into the final plugin.
One particular problem that we ran into was the [rate limiting](https://developer.github.com/v3/#rate-limiting) set by the GitHub API which meant that a user could only make 60 requests per hour to this API. This could be alleviated by providing an access token, thus bumping the rate limit up to 5000. Even so, there might have been cases where users wouldn't want to provide an access token.

To tackle this problem we added in an auto refresh config option to the plugin. Users could choose how often the plugin would ping GitHub for updates or turn off this functionality entirely. We also added a method to check for updates manually that could be used when auto refresh is off.
Our [project README](https://github.com/uberVU/github-changelog/blob/master/README.md) goes into further detail about the tech we’re using and all of the config options available to the plugin.

**Wrapping it all up:** Once we had everything well defined, writing the actual code was a breeze. Because there were little unknowns it was easy to focus on just what needed to be built. Any issues that did arise were small and easily fixed . Within a couple of days we had a fully working version [ready to demo](http://ubervu.github.io/github-changelog/).

##The future
Our plugin, github-changelog, has been open-sourced from the start and we're open to anyone who wants to help us make it even better. Check out our GitHub repo and tell us what you think.
Github-changelog is part of our product and we will continue to maintain and evolve it.

[1]: https://github.com/uberVU/github-changelog/

