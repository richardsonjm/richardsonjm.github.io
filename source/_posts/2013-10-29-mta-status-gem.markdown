---
layout: post
title: "mta_status gem"
date: 2013-10-29 23:29
comments: true
categories: 
---
###Should I run for the train?


![parkour](http://farm8.staticflickr.com/7324/10576880723_3ae95bb861_o.jpg)


Whenver I need to catch the [LIRR](http://lirr42.mta.info/), I usually end up running. And If I'm dodging and weaving around the fine people of NYC, I hope there's a train warmed up and ready to go when I get to the station. Given that I spend so much time in the terminal (...on my computer) these days, I decided to build a gem that makes sure everything is ok with my train before the race begins by pulling the latest train service status data off the MTA webiste. 

The gem can be installed by typing the following into your terminal:

	gem install mta_status
	
Type in the following command into your terminal and you will get the subway service status by default. 

	mta_status 
	
If you type lirr, metro-north, or a host of other related terms after the mta_status command you should get the service status for that particular branch of the MTA. For example, the following will get you the service status for the LIRR.

	mta_status lirr 


###Data source

The link to the train service [status](http://web.mta.info/status/serviceStatus.txt) page can be found on the MTA [MTA] developer [downloads](<http://web.mta.info/developers/download.html>) page. 

In addition to service status, the developer site also has links to schedules and route information all meant to follow the General Transit Feed Specification (GTFS) [specs](https://developers.google.com/transit/gtfs/). In other words, if you build something for one transit system, it will likely be transferrable to one of these [other](https://code.google.com/p/googletransitdatafeed/wiki/PublicFeeds) transits systems in the US and through the rest of the world. 


###Implementation

The MTA service [status](http://web.mta.info/status/serviceStatus.txt) page is a text file with no css. In my previous (and limited) expierence with Nokogiri I had searched for nodes using css, so with this page I had to search for nodes using xpath. 

	# xpath
	@doc.xpath("//name").collect do |name|
      name.children.text 
	
	# css
	student_page.css('div.social-icons a').collect do |link|
        link.attr('href')
        
The page was layed out well so it didn't take too long to figure out how to get xpath to find what I was looking for. Let me rephrase that, the page was pretty well laid out expcept for this:

    <text>
                    &lt;span class="TitlePlannedWork" &gt;Planned Work&lt;/span&gt;
                     &lt;span class="DateStyle"&gt;
                    &amp;nbsp;Posted:&amp;nbsp;10/29/2013&amp;nbsp; 9:43AM
                    &lt;/span&gt;&lt;br/&gt;&lt;br/&gt;
                  &lt;a class="plannedWorkDetailLink" onclick=ShowHide(50159);&gt;
	&lt;b&gt;Busing at Melrose and Tremont Stations
	&lt;/a&gt;&lt;br/&gt;&lt;br/&gt;&lt;div id= 50159 class="plannedWorkDetail" &gt;&lt;/b&gt;Beginning Aug 19 until further notice
	&lt;br&gt;
	&lt;br&gt;As part of the Bronx Right-of-Way Improvements Project, substitute bus service
	&lt;br&gt;will be provided at Melrose and Tremont Stations in both directions.  Buses will
	&lt;br&gt;connect with trains at Fordham Station. Please visit &lt;a href=http://new.mta.info/mnr target=_blank&gt;&lt;font color=#0000FF&gt;&lt;b&gt;&lt;u&gt;mta.info&lt;/u&gt;&lt;/b&gt;&lt;/font&gt;&lt;/a&gt; for further details.
	&lt;br&gt;
	&lt;br&gt;Weekday Bus Schedule  |  &lt;a href=http://advisory.mtanyct.info/pdf_files/HARLEM_08-19-2013%20FINAL%20Buses%20Only%20Mon-Fri.pdf target=_blank&gt;&lt;font color=#0000FF&gt;&lt;b&gt;&lt;u&gt;pdf&lt;/u&gt;&lt;/b&gt;&lt;/font&gt;&lt;/a&gt;
	&lt;br&gt;Weekend Bus Schedule  |  &lt;a href=http://advisory.mtanyct.info/pdf_files/HARLEM_08-19-2013%20FINAL%20Buses%20Only%20Sat-Sun.pdf target=_blank&gt;&lt;font color=#0000FF&gt;&lt;b&gt;&lt;u&gt;pdf&lt;/u&gt;&lt;/b&gt;&lt;/font&gt;&lt;/a&gt;
	&lt;br&gt;&lt;b&gt;
	&lt;br&gt;&lt;/div&gt;&lt;/b&gt;&lt;br/&gt;
                &lt;br/&gt;&lt;br/&gt;
              </text>

This is the "text" that explains the service disruption. My next task is to make sense of this mess and then potentially work it into a new version of the gem. Only issue there is the more features I add, the less useful the gem could become, especially since the service change information is often confusing.


###TODO
I get the following gem related error everytime the gem runs. It doesn't seem to effect how the gem runs, but it is ugly. 

	WARN: Unresolved specs during Gem::Specification.reset:
      	mini_portile (~> 0.5.0)
	WARN: Clearing out unresolved specs.
	Please report a bug if this causes problems.

Also, there's a ton of information on the bus system that I didn't incorporate and also some information on the bridges and tunnels operated by the MTA. 
