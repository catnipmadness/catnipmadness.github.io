---
layout: post
title:  "What data is available from eBird?"
date:   2020-01-03 21:55:00 +1300
categories: nzatlas ebird data datasets
background: '/images/IMG_8459.JPG'
---

The NZ Bird Atlas website already provides good data products and visualisations, with tables, interactive maps and personal birding effort statistics. The most compelling visuals are the [atlas effort maps](https://ebird.org/atlasnz/effortmap "Atlas Effort Maps"){:target="_blank"} for New Zealand. These provide a quick assessment of effort, by grid square, across the entire country, broken down by season, as well as a running tally nationally.

As a visualisation of effort across New Zealand, these maps are a great tool, and the motivation is to turn the squares yellow (or darker colours indicating more cummulative effort in a grid square) by completing checklists within the boundaries of each grid. This visualisation, unfortunately, also has a serious drawback - it provides no indication as the completeness of habitat coverage within a square, and no motivation to go and fill in the blanks for those habitats.

This lead me down a path of trying to download the data myself, and use R and QGIS to process and analyse the data for my own attempt at establishing where the blanks were from a habitat perspective. 

This post covers getting that data from the [NZ Bird Atlas website](https://ebird.org/atlasnz "NZ Bird Atlas"){:target="_blank"}).

Finding data to download and visualise from the NZ Bird Atlas website 
([eBird data products](https://ebird.org/atlasnz/science/download-ebird-data-products "eBird data products"){:target="_blank"}) takes a little time. The first port of call is through the Science section of the website, which provides instructions on downloading:

- eBird Basic Dataset (EBD)
- eBird Status and Trends
- eBird Observational Dataset (EOD)
- Accessing your personal eBird data

This last item was a bit confusing as it stated:

> To download your own data go to **My eBird** and then to ‘Download my data’ on the right side ... 

The options on the right-hand side of the [My eBird page](https://ebird.org/atlasnz/myebird "My eBird"){:target="_blank"} for the NZ Bird Atlas looks like:

![atlasnz-my-eBird]({{site.url}}/images/ebird-20200103-atlasnz-my-ebird.png "My eBird menu on right-hand side of page")

There is no 'Download my data' option! 

However, the right-hand side of the default [My eBird page](https://ebird.org/atlasnz/myebird "My eBird"){:target="_blank"}, does.

![atlasnz-my-eBird]({{site.url}}/images/ebird-20200103-my-ebird.png "My eBird menu on right-hand side of page")

Clicking the link takes you to the download page, where you are prompted to click a button in order to get a zip file mailed to your inbox (based on the email you provided when you registered with eBird).

![download-my-data]({{site.url}}/images/ebird-20200103-download-my-data.png "Download my data")
