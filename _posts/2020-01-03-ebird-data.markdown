---
layout: post
title:  "What data is available from eBird?"
date:   2020-01-03 21:55:00 +1300
categories: nzatlas ebird data datasets
background: '/images/IMG_8459.JPG'
---

The NZ Bird Atlas website provides good data products and visualisations, with tables, interactive maps and personal birding effort statistics. The most compelling visuals are the [atlas effort maps](https://ebird.org/atlasnz/effortmap "Atlas Effort Maps"){:target="_blank"} for New Zealand. These provide a quick assessment of effort, by grid square, across the entire country, broken down by season, as well as a running tally nationally.

As an indicator of effort across New Zealand, these maps are a great tool, and the motivation is to turn the squares yellow (or darker colours indicating more cummulative effort in a grid square) by completing checklists within the boundaries of each grid. This particular style of visualisation, unfortunately, also has a serious drawback - it provides no indication as to the completeness of habitat coverage within a square. This may have been a deliberate choice when the website was set up, with initial effort focussed on getting people involved, without too much complication in what the effort map was showing.

This led me down a path of trying to download the data myself, using R and QGIS to process and analyse the data, in an attempt at establishing where the blanks were from a habitat perspective. 

This post covers getting that data from the [NZ Bird Atlas website](https://ebird.org/atlasnz "NZ Bird Atlas"){:target="_blank"}).

Finding data to download and visualise from the NZ Bird Atlas website 
([eBird data products](https://ebird.org/atlasnz/science/download-ebird-data-products "eBird data products"){:target="_blank"}) takes a little time. The first port of call is through the Science section of the website, which provides instructions on downloading:

- eBird Basic Dataset (EBD)
- eBird Status and Trends
- eBird Observational Dataset (EOD)
- Accessing your personal eBird data

This last item was a bit confusing as it stated:

> To download your own data go to My eBird and then to ‘Download my data’ on the right side ... 

The options on the right-hand side of the [My eBird page](https://ebird.org/atlasnz/myebird "My eBird"){:target="_blank"} for the NZ Bird Atlas looks like:

![atlasnz-my-eBird]({{site.url}}/images/ebird-20200103-atlasnz-my-ebird.png "My eBird menu on right-hand side of page")

There is no 'Download my data' option! 

This is what generated the confusion - one part of the website said go here to download, but there was no download "there". It was then I realised there were two "eBird" websites - the **default** one which focussed mainly on North American birding, and the NZ Bird Atlas which is a site built on top of the default one. These two sites have different options and different visualisations. It all became clear. 

The download my data option exists on the **default** [My eBird page](https://ebird.org/atlasnz/myebird "My eBird"){:target="_blank"}, but not on the NZ Bird Atlas site.

![atlasnz-my-eBird]({{site.url}}/images/ebird-20200103-my-ebird.png "My eBird menu on right-hand side of page")

Now that I had found the download link, I could get somewhere. Clicking the link takes you to the download page, where you are prompted to click a submit button in order to get a zip file mailed to your inbox (based on the email you provided when you registered with eBird).

![download-my-data]({{site.url}}/images/ebird-20200103-download-my-data.png "Download my data")

This zip file contains a .csv file of all the checklist data **you** have collected (or has been shared with you) and includes the lat-long coordinates, along with dates, times and places, species id's and counts - perfect for putting on a map, and for combining with other national datasets such as the landcover database.

My next post will describe processing this data in preparation for mapping (as there are a few gotcha's along the way).

What is still not adressed at this point is what other birders are collecting for the NZ Bird Atlas. As a download only contains your individual data, it is difficult to assess what others have done, save for the tables and maps on the website. This will be covered later, also.