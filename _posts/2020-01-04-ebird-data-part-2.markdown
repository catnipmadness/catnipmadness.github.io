---
layout: post
title:  "Looking at your eBird data?"
date:   2020-01-04 15:20:00 +1300
categories: nzatlas ebird data datasets
background: '/images/IMG_8459.JPG'
---

>My next post will describe processing this data in preparation for mapping (as there are a few gotcha's along the way).

... continued

The csv file received will look like this:
![csv data]({{site.url}}/images/ebird-20200104-csv-data.png "data file")
... or if opening a spreadsheet, more like this:
![csv data]({{site.url}}/images/ebird-20200104-ss-data.png "data file")

The important thing here is to understand the purpose of each of the columns in this file. 

Column Name	| Description 
:--------------|:------------
Submission ID |	Unique identifier for the given checklist. Note: any shared checklist will get its own Submission ID
Common Name | Bird common name
Scientific Name | Bird Scientific name
Taxonomic Order	| Numeric value which refers to species taxonomic order, and found in [Clements Integrated Checklist](https://www.birds.cornell.edu/clementschecklist/overview-august-2019/){:target="_blank"}, available from eBird site
Count | Species count as recorded by the observer
State/Province | Used to identify NZ region where checklist captured, e.g. NZ-MWT, NZ-WKO, NZ-BOP
County | This is the name of the city or district area - based on Territorial Authorities
Location ID | Unique identifier given to a checklist location
Location | Location name provided by the observer
Latitude | Geographic coordinates - latitude - EPSG 4326
Longitude | Geographic coordinates - longitude - EPSG:4326
Date | Checklist date (YYYY-MM-DD)
Time | Checklist time (HH:MM AM/PM)
Protocol | Checklist sampling protocol. One of: "ebird - Casual Observation" OR "ebird - Stationary Count" OR "ebird - Travelling Count"
Duration (Min)|	Time duration, in minutes, of checklist observation
All Obs Reported|	Numeric 0 or 1. Indicates in complete checklist collected. 1 = Yes, 0 = No
Distance Traveled (km)|	Used for travelling checklists to record distance in kilometres
Area Covered (ha)|	Area covered (hectares) is not available through the mobile eBird app
Number of Observers|	Defaults to 1, unless the checklist is shared. Then the number is based on how many people the checklist is shared with.
Breeding Code|	This comes from the drop down list of breeding codes available in the mobile app, or on the website.
Observation Details|	Observer comments on individual species recorded on checklist
Checklist Comments|	Observer comments on whole checklist
ML Catalog Numbers|	[Macaulay Library](https://www.macaulaylibrary.org){:target="_blank"} catalog number for media (audio, photo). These get updated as additional media are added against a species observation

These fields/columns are based on the settings of the default ebird website, rather than the NZ Bird Atlas portal (see note and the bottom of this post). This means that it contains all the checklists that you, as an observer, have collected - no matter when, and no matter where. The NZ Bird Atlas project only applies to the 2019-2024 time period within New Zealand.

Based on this, some filtering needs to occur on the downloaded file to ensure that only New Zealand observations, within that timeframe, are considered. Simple. Just use the `State/Province` and `Date` fields to filter the list!

So, assuming table is just called `csv`, a pseudo-SQL might look like:
```sql
SELECT LEFT('State/Province',2) as country, CAST('Date' AS Date) as isoDate
  FROM  csv 
  WHERE country = 'NZ' AND
        isoDate >= '2019-06-01'
```

All good right?

... Not so fast

Remember the question about "Are you submitting a complete checklist ..."? Your user statistics are based on the data that relate to complete checklists only. This value is stored in the `Protocol` column, and can take three possible values:
- ebird - Casual Observation
- ebird - Stationary Count
- ebird - Travelling Count

From my interpretation of the data, irrespective of whether you are collecting a checklist as a stationary or travelling count, but do not mark it as a complete checklist, the "Protocol" will be recorded as "ebird - Casual Observation". The other two values represent complete checklists, for either stationary or travelling counts.

OK. So the selection criteria will now be something like:

```sql
SELECT LEFT('State/Province',2) as country, CAST('Date' AS Date) as isoDate
  FROM  csv 
  WHERE country = 'NZ' AND
        isoDate >= '2019-06-01' AND
        Protocol != "ebird - Casual Observation"?
```

Good to go now?

Sort of. This is sufficient to get you started with NZ Bird Atlas data at the with respect to checklists and locations. There are still some issues around taxonomic orders (some odd duplicate listings exist in the species lists, but these can resolved by reference back to the [Clements Integrated Checklist](https://www.birds.cornell.edu/clementschecklist/overview-august-2019/){:target="_blank"}).

I'll discuss species identification in a later post and come back to this point there.

With this information, it is now possible to load the data into your tool of choice, make your data selections based on what you have in your checklists, and prepare maps or other visualisations of your choice.

In my next post, I will talk about how to use the R Scripting language to process these data for the purposes of understanding checklist effort and habitat coverage, by season.

------

NOTE: There are more than just two versions of the website. Portals have been built on top of eBird site, based on the region of the world you are in, or on what project is being supported by Cornell University, as is seen when configuring the mobile eBird app to use the NZ Bird Atlas portal.

![Mobile app portal selection]({{site.url}}/images/IMG_8666.PNG "Portal selection")




