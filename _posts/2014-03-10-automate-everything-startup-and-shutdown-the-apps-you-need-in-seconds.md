---
title: 'Automate everything &#8211; startup and shutdown the apps you need in seconds'
author: Alex Palcuie
layout: post
permalink: /automate-everything-startup-and-shutdown-the-apps-you-need-in-seconds/
categories:
  - Tips and Tricks
tags:
  - development
  - open source
  - puppet
  - vagrant
meta: http://code.hootsuite.com/automate-everything-startup-and-shutdown-the-apps-you-need-in-seconds
---
Do you hate coming at the office, firing up your favourite text editor, getting ready to do some work and&#8230; it you forgot to start your Vagrant machine? Now you have to open up the terminal, type *vagrant up*, wait 90+ seconds for the beast to load and.. it doesn&#8217;t work because you forgot to to connect to the VPN and Puppet cannot correctly provision the box without this. You do that also.

Now that everything is setup &#8211; you code the entire day, pack your things, arrive home, start browsing threads on Hacker News and&#8230; the low battery warning comes up because you forgot to close Vagrant.

There are just too many irrelevant little tasks we need to keep in our  brain cache and there is precious little space in it for useless, repetitive information. Whatever you work with, all of us have these little tasks we do day in and day out that only waste time and demand energy.

For those of you on OSX, there is something that can help. I made <a href="https://github.com/uberVU/toolbox/tree/master/applescript" target="_blank">some tiny little AppleScripts</a> that solve these annoying problems.

<a href="https://xkcd.com/1319/" target="_blank"><img class="aligncenter size-full wp-image-164" alt="automation" src="{{ site.url }}/images/wordpress/2014/03/automation.png" width="404" height="408" /></a>

The before work script contains two parts. One for opening a list of apps found in *apps.txt* and another  one for running your custom scripts.

<pre class="brush: applescript; title: ; notranslate" title="">set current_path to POSIX path of ((path to me as text) & "::")
-- get work apps
set appList to {}
set myApps to paragraphs of (read current_path & "apps.txt")
repeat with nextLine in myApps
 if length of nextLine is greater than 0 then
  copy (nextLine as string) to the end of appList
 end if
end repeat

-- open work apps
repeat with i in appList
 tell application i to activate
end repeat

-- run other scripts from folder
set scriptList to {"connect_tunnelblick", "open_vagrant", "open_chrome_tabs"}
repeat with i in scriptList
 run script current_path & i & ".applescript"
end repeat</pre>

The open_vagrant script just opens an iTerm session and runs a vagrant up command:

<pre class="brush: applescript; title: ; notranslate" title="">set open_vagrant to "z puppet; vagrant up;"

tell application "iTerm"
 activate
 try
  set mysession to current session of current terminal
 on error
  set myterm to (make new terminal)
  tell myterm
   launch session "Default"
   set mysession to current session
  end tell
 end try

 tell mysession
  write text open_vagrant
 end tell
end tell</pre>

AppleScript is a pretty funny language to code in, but it&#8217;s still rough around the edges. For example to close iTerm you first have to ignore the system events, then issue the exit for the app, catch system events and finally press enter.

<pre class="brush: applescript; title: ; notranslate" title="">set close_vagrant to "z puppet; vagrant halt -f;"

-- close vagrant
tell application "iTerm"
 activate
 set myterm to (make new terminal)
 tell myterm
  launch session "Default"
  set mysession to current session
 end tell

 tell mysession
  write text close_vagrant
 end tell

 delay 5
end tell

-- quit iTerm
ignoring application responses
 tell application "iTerm"
  quit
 end tell
end ignoring

delay 2
tell application "System Events" to keystroke return</pre>

Is it worth the time? XKCD has the answer:

<a href="http://xkcd.com/1205/" target="_blank"><img class="aligncenter size-full wp-image-171" alt="is_it_worth_the_time" src="{{ site.url }}/images/wordpress/2014/03/is_it_worth_the_time.png" width="571" height="464" /></a>

Given that I do these things daily and I may save between 1 and 5 minutes per day, then I save between 1 and 6 days per 5 years. It doesn&#8217;t look too much, but given that time is <a href="http://www.quora.com/What-are-some-important-and-generalizable-life-lessons/answer/Yishan-Wong" target="_blank">highly, uniquely and equitably limited</a>, we should strive doing our best in squashing annoying things that chop it.


**Alternative**: got a tip on <a href="https://www.facebook.com/palcuiealex/posts/10201097721599776?stream_ref=10" target="_blank">Facebook</a> about <a href="https://github.com/tmuxinator/tmuxinator" target="_blank">Tmuxinator</a> which automates opening programs in your terminal. Also, put the scripts somewhere searchable by Spotlight so you can launch them easily, or use my [Alfred workflow][1].

***About the author**: I love reading, writing and spreading valuable stories. You should follow me on <a href="http://twitter.com/AlexPalcuie" target="_blank">Twitter</a>, <a href="http://facebook.com/palcuiealex" target="_blank">Facebook</a> and <a href="http://google.com/+alexpalcuie" target="_blank">Google+</a>. Check out other cool projects I made on <a href="http://github.com/palcu" target="_blank">Github</a>.*

<div class="addtoany_share_save_container addtoany_content_bottom">
  <div class="a2a_kit a2a_kit_size_32 addtoany_list a2a_target" id="wpa2a_10">
    <a class="a2a_button_facebook" href="http://www.addtoany.com/add_to/facebook?linkurl=http%3A%2F%2Fdev.ubervu.com%2Fautomate-everything-startup-and-shutdown-the-apps-you-need-in-seconds%2F&linkname=Automate%20everything%20%E2%80%93%20startup%20and%20shutdown%20the%20apps%20you%20need%20in%20seconds" title="Facebook" rel="nofollow" target="_blank"></a><a class="a2a_button_twitter" href="http://www.addtoany.com/add_to/twitter?linkurl=http%3A%2F%2Fdev.ubervu.com%2Fautomate-everything-startup-and-shutdown-the-apps-you-need-in-seconds%2F&linkname=Automate%20everything%20%E2%80%93%20startup%20and%20shutdown%20the%20apps%20you%20need%20in%20seconds" title="Twitter" rel="nofollow" target="_blank"></a><a class="a2a_button_google_plus" href="http://www.addtoany.com/add_to/google_plus?linkurl=http%3A%2F%2Fdev.ubervu.com%2Fautomate-everything-startup-and-shutdown-the-apps-you-need-in-seconds%2F&linkname=Automate%20everything%20%E2%80%93%20startup%20and%20shutdown%20the%20apps%20you%20need%20in%20seconds" title="Google+" rel="nofollow" target="_blank"></a><a class="a2a_dd addtoany_share_save" href="http://www.addtoany.com/share_save"></a>
  </div>
</div>

 [1]: https://github.com/uberVU/toolbox/blob/master/applescript/ubervu_applescripts.alfredworkflow