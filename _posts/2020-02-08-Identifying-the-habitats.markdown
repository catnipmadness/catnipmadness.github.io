---
layout: post
title:  "Identifying habitats for checklists"
date:   2020-02-08 12:00:00 +1300
categories: nzatlas ebird birding checklists habitats lcdb
background: '/images/IMG_7307.JPG'
---

### The quick read
Being able to identify all the main habitats in a bird atlas grid square is challenging. Looking at the aerial photos of New Zealand on the eBird website is a good place to start, but it can be difficult to differentiate between distinct vegetation types at times. 

Using the Manaaki Whenua Landcare Research Land Cover Database (LCDB) simplifies this problem immensely. It becomes easy to (a) distinguish landcover types, and (b) it helps to identify what the habitat types actually are.

With the release of version 5 of this database at the beginning of 2020, it's an opportunity to use the most recent data to help guide birding effort.

Cutting to the chase today, the latest version of the LCDB can be viewed at this [website](https://vizbe.landcareresearch.co.nz){:target="_blank"}, alongside being able to look at the changes over time.

[![Manaaki Whenua Landcover Explorer]({{site.url}}/images/LCDB-Explorer.PNG "Manaaki Whenua Landcover Explorer")](https://vizbe.landcareresearch.co.nz){:target="_blank"}

Go and check out the areas where you collect NZ Bird Atlas checklists to see what changes have been recorded.

---

### The slightly longer read

Todays blog post will consider the issue of habitat in a bit more detail. To start, the following statements come from the [NZ Bird Atlas Handbook](https://birdatlas.co.nz/assets/0ee293e775/New-Zealand-Bird-Atlas-Handbook-version-1.pdf "PDF Handbook"){:target="_blank"}).:

#### Goal
>"Over the next five years, the goal is to survey each and every grid square, and to compile at least one checklist of all of the bird species encountered within each **major habitat type** within each square, during each of the four seasons ... "

#### Method
>"Familiarise yourself with the grid square that you’ve chosen to survey, including identifying all the **major habitat types** (e.g. beech forest, plantation forest, manuka shrubland, rivers, wetlands, suburban habitats, sandy beaches, rocky shorelines, etc) within the grid square. Ideally, participants should aim to visit each major habitat type within a square, and complete at least one complete checklist per habitat type ..." 

#### Habitat source data
>"Observers do not need to routinely collect habitat data for individual checklists. Instead pre-existing datasets, such as the **Land Cover Database, will be used to generate the habitat data** required for the Atlas analysis. However, observers should note any “ephemeral” habitat or vegetation types that they find themselves surveying. These are habitat types which only last for a very short time: e.g annual crops, ploughed fields or recently-harvested plantation forests. These “temporary” habitats tend not to be very easily captured in pre-existing habitat datasets."

When we started data collection for the NZ Bird Atlas, the map information used to determine habitats was LCDB version 4.1. In early 2020, [version 5.0](https://lris.scinfo.org.nz/layer/104400-lcdb-v50-land-cover-database-version-50-mainland-new-zealand/ "LCDB v5.0"){:target="_blank"} was released.

Documentation for v5.0, available from the link above, describes how it has been improved from previous versions:

> - Correction of error (in classification or delineation) some notified by users, some logged post-LCDB v4.1 by Landcare Research and some observed during mapping by analysts. In particular, a significant area of Stewart Island from the head of Patterson Inlet, up the Ruggedy Flat and through to Mason Bay was remapped to correct earlier mapping.
> - Mapping of detected change between 2012/13 and 2018/19 including hitherto undetected ponds, wetland loss, urban expansion, and change associated with harvesting/replanting of production forests
> - A review of impossible and implausible land cover transitions, correcting those found in error 
> - Re-mapping of built-up areas in Canterbury to revert earthquake 'red-zone' areas, mostly to 'Urban Open Space', using Environment Canterbury data
> - Improving identification and tracking of wetlands by creating a wetland identifier for every time-step and expanding it's use to include all wetlands (but not open water) irrespective of whether it is implied by the class name or not. 
> - Installing a capacity to map and manage significant coastline change by, firstly incorporating a recent higher-fidelity coast from Topo50 ("NZ Coastlines and Islands Polygons (Topo 1:50k)" dataset, downloaded from LINZ on 16 April 2019) and then selecting and screening areas of significant difference (width > 50 m and area > 1 ha) to map 22 areas of verified change since the previously downloaded (May 2012) LINZ coastline used in earlier LCDB versions. This change, any future change, and any re-worked earlier change can now be mapped and managed by 'Onshore' indicators for every time-step in LCDB.
> - Incorporation of high-quality wetland mapping of dispersed locations nationally arising from an MfE-contracted review of wetland loss and, a comprehensive set of high-fidelity wetland polygons created from reference to Environment Waikato data.
> - Application of [MfE's irrigation layer](https://data.mfe.govt.nz/layer/90838-irrigated-land-area-2017/ "MFE's irrigation layer"){:target="_blank"} to identify areas of low producing , depleted, and tussock grassland that should be mapped as high producing exotic grassland.

As you would have seen in previous blog posts, we had been applying a method to visualise the remaining habitats in grid squares that still needed checklists collected, and this method utilised LCDB v4.1. 

Now that v5.0 is out, I have updated the underlying data used to prepare this visualisation. Relying on a copper connection to the internet, and a 2014 MacBook Pro, the new landcover database was downloaded (~700mb) and intersected with the bird atlas grid squares (the data were also dissolved based on grid square name and the land cover type). With the data processed, it was then uploaded to a database in the cloud (on AWS) so that it is accessible anywhere, rather than just being stuck on my computer. This process took most of day, the bulk of which was spent waiting for it to upload to the cloud.

Now that is done, all new visualisations, and any tools built for others to access and use, will be based upon LCDB v5.0.

A final reflection on the handbook instructions concerning "major habitat types". Our checklist effort to date has been about trying to collect lists in ALL the habitat types as defined by the LCDB. If we were to only consider "major habitat types", the effort required (a) could be reduced slightly, and (b) redirected to cover more grid squares. Whether or not we adopt this approach is still subject to some discussion, but I would argue that as time is limiting factor, making the most of the time we have to just sample the major habitat types is the best approach. 

Don't forget to go and look at the Landcover explorer. Until next time.

[![Manaaki Whenua Landcover Explorer]({{site.url}}/images/LCDB-Explorer.PNG "Manaaki Whenua Landcover Explorer")](https://vizbe.landcareresearch.co.nz){:target="_blank"}
