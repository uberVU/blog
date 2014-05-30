##The uberVU Engineering Blog

This repository is here to help us organize our dev blog.
Add issues related to the blog and publish directly by using it.

The blog is hosted using [Github Pages](https://pages.github.com/).
The address of the blog is [http://ubervu.github.io/blog/](http://ubervu.github.io/blog/)

###Github Pages and Jekyll

The code for the blog lives in this repo on the branch `gh-pages`. Whatever is
on that branch will be served and be visible at [http://ubervu.github.io/blog/](http://ubervu.github.io/blog/).

The blog is generated using Jekyll which you can download [here](http://jekyllrb.com/).
This tool generates a static website from a bunch of simple .md files and some
template files. When something new is pushed to the `gh-pages` branch the whole static site is regenerated.

To work locally with the pages you can use any Markdown editor to make the new
blog post files. The blog posts are kept in [_posts](https://github.com/uberVU/blog/tree/gh-pages/_posts)
folder. Check out the Jekyll documentation for more details on the templating system and
the structure of the site.

You can also start Jekyll and serve the pages from a local server by running
`jekyll serve --watch --config _config.yml,_config_dev.yml` inside the blog
repo while on the `gh-pages` branch or another one started from it.
You also need to pass the `_config_dev.yml` file for the links to be ok.

###The flow of publishing a new post
To make it easier use the same flow as with any issue. Make an issue, create
a branch for the issue, add the new post in the branch, make a pull request,
get it reviewed, if needed, and merge the pull request in gh-pages. That's it.
