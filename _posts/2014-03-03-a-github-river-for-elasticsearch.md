---
title: A GitHub River for Elasticsearch
author: Mihnea Dobrescu-Balaur
layout: post
permalink: /a-github-river-for-elasticsearch/
thesis_thumb_width:
  - 66
thesis_thumb_height:
  - 66
categories:
  - Our projects
tags:
  - elasticsearch
  - github
  - Ope source
---
We&#8217;ve started using [Elasticsearch][1] for a few of our projects. It&#8217;s a great tool for storing and, especially, querying  giant text datasets. In the words of their creators &#8220;Elasticsearch is a flexible and powerful open source, distributed, real-time search and analytics engine&#8221;. It&#8217;s fast and it&#8217;s ease to use and very useful in many situations.

One of the things that Elasticsearch does very well is to listen to a large stream of data and index it. This is done very easily with what&#8217;s called a [river][2]. A river is an easy way to set up a continuous flow of data that goes into your Elasticsearch datastore. [Quoting][3] once more the creators &#8220;A river is a pluggable service running within Elasticsearch cluster pulling data (or being pushed with data) that is then indexed into the cluster.&#8221;

It is more convenient than the classical way of manually indexing data because once configured, all the data will be updated automatically. This reduces complexity and also helps build a real-time system.

There are already a few rivers out there, among which are:

*   Twitter River Plugin for ElasticSearch ([link][4])
*   Wikipedia River Plugin for Elasticsearch ([link][5])

There was no GitHub river, though. So we setup to build our own. And thus Elasticsearch GitHub river was born: <https://github.com/ubervu/elasticsearch-river-github>.

We&#8217;re big GitHub fans and use it extensively to manage our daily activities. Sometimes we find ourselves looking at heaps of issues that scream for attention. To avoid this pile up we want to understand better how our team works, how issues end up being forgotten, how to address the real important ones first. The GitHub river was built to give us this additional insight we crave for. And hopefully it will help you too.

Now lets get down do business and see how to use it

### Using the GitHub river

The GitHub river allows us to periodically index a repository&#8217;s  events. The repo&#8217;s can be both public and private. If you have a private repository you&#8217;ll need to provide [authentication data][6].

To get a taste of the workflow possibilities this opens, let&#8217;s index some data from another open source project we like, use and contribute to: [Lettuce][7] and  lets explore it in a pretty [Kibana][8] dashboard (click on the image to get the full version).

<a href="{{ site.url }}/images/wordpress/2014/02/gh-kibana-dashboard.png" target="_blank"><img class="wp-image-149 aligncenter" alt="gh-kibana-dashboard" src="{{ site.url }}/images/wordpress/2014/02/gh-kibana-dashboard.png" width="494" height="636" /></a>

Assuming you have [Elasticsearch][9] already installed, you first need to install the [river][10]. Make sure you restart elasticsearch after this so it picks up the new plugin.

Now we can create the GitHub river:

<pre>curl -XPUT localhost:9200/_river/gh_river/_meta -d '{
    "type": "github",
    "github": {
        "owner": "gabrielfalcao",
        "repository": "lettuce",
        "interval": 3600
    }
}</pre>

Right after this, Elasticsearch will start indexing the most recent 300 public events of *gabrielfalcao/lettuce*. The 300 events limit is imposed by the GitHub API policy. After one hour, it will check again for new events.

The data is accessible in the *gabrielfalcao-lettuce* index, where you will find a different document type for every GitHub event type.

### Using Kibana to visualize the data

Great, we have some data, what next? In order to make some sense of it, let&#8217;s get Kibana up and running. First, you need to [download][11] and extract the latest Kibana build. To access it, open *kibana-latest/index.html* in your favorite web browser.

What you see now is the default Kibana start screen. You could go ahead and configure your own dashboard, but to speed things up we suggest you import the dashboard we&#8217;ve set up. First, download the JSON [file][12] that defines the dashboard. Then, at the top-right corner of the page, go to *Load > Advanced > Choose file* and select the downloaded JSON.

That&#8217;s it! You now have a basic dashboard set up that shows some key graphs based on the GitHub data you have in elasticsearch. Furthermore, thanks to the river and the way the dashboard is set up, you will get new data every hour and the dashboard will refresh accordingly.

Happy hacking!

### A word about open source.

We&#8217;re big fans of open source. We use many great open source products and also do our best to give back as much as we can. Checkout out the uberVU GitHub [page][13] for some of the project we&#8217;ve built. We&#8217;re constantly working and contributing to our own projects or new projects. If you want to contribute to our projects but don&#8217;t know where to start or you think that we can help a project in any way let us know!

<div class="addtoany_share_save_container addtoany_content_bottom">
  <div class="a2a_kit a2a_kit_size_32 addtoany_list a2a_target" id="wpa2a_9">
    <a class="a2a_button_facebook" href="http://www.addtoany.com/add_to/facebook?linkurl=http%3A%2F%2Fdev.ubervu.com%2Fa-github-river-for-elasticsearch%2F&linkname=A%20GitHub%20River%20for%20Elasticsearch" title="Facebook" rel="nofollow" target="_blank"></a><a class="a2a_button_twitter" href="http://www.addtoany.com/add_to/twitter?linkurl=http%3A%2F%2Fdev.ubervu.com%2Fa-github-river-for-elasticsearch%2F&linkname=A%20GitHub%20River%20for%20Elasticsearch" title="Twitter" rel="nofollow" target="_blank"></a><a class="a2a_button_google_plus" href="http://www.addtoany.com/add_to/google_plus?linkurl=http%3A%2F%2Fdev.ubervu.com%2Fa-github-river-for-elasticsearch%2F&linkname=A%20GitHub%20River%20for%20Elasticsearch" title="Google+" rel="nofollow" target="_blank"></a><a class="a2a_dd addtoany_share_save" href="http://www.addtoany.com/share_save"></a>
  </div>
</div>

 [1]: http://www.elasticsearch.org/
 [2]: http://www.elasticsearch.org/blog/the-river/
 [3]: http://www.elasticsearch.org/guide/en/elasticsearch/rivers/current/index.html
 [4]: https://github.com/elasticsearch/elasticsearch-river-twitter/
 [5]: https://github.com/elasticsearch/elasticsearch-river-wikipedia/blob/master/README.md
 [6]: https://github.com/uberVU/elasticsearch-river-github/blob/master/README.md
 [7]: https://github.com/gabrielfalcao/lettuce
 [8]: http://www.elasticsearch.org/overview/kibana/
 [9]: http://www.elasticsearch.org/overview/elkdownloads/
 [10]: http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/modules-plugins.html
 [11]: http://download.elasticsearch.org/kibana/kibana/kibana-latest.zip
 [12]: http://data.mihneadb.net/gh-kibana.json
 [13]: https://github.com/ubervu/