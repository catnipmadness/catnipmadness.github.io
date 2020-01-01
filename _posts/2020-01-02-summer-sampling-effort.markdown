---
layout: post
title:  "Summer sampling effort to date"
date:   2020-01-01 22:05:00 +1300
categories: nzatlas ebird mapbook GIS RStudio
background: '/images/IMG_8535.JPG'
---

So, how have we been assessing our sampling effort?

In short, at the end of each days collection of checklists, we download our data from the eBird website (a later post might cover this topic, but it is reasonably straight-forward), process it through an R script, and then map it using the free and opensource GIS tool, [QGIS](https://qgis.org "QGIS homepage"){:target="_blank"}.

The net result, is a searchable map that allows us to look at checklist locations, within the NZ Atlas grid sqaures, and to identify habitats that remain to be sampled (based on the landcover database, LCDB, v4.1). It should be noted that the LCDB v4.1 was current as at 2012, so is a little out of date.

QGIS Map Output: ![QGIS Map]({{site.url}}/images/qgis-20200101-sampling-effort.png "QGIS Map Output"){: width=66% padding:16px"}

