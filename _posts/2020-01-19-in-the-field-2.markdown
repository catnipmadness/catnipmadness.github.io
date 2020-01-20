---
layout: post
title:  "Routinely collecting checklists Pt 2"
date:   2020-01-20 16:20:00 +1300
categories: nzatlas ebird birding checklists
background: '/images/IMG_8084.JPG'
---

Forest Hill Road, located between the Manawatu River and Tararua Ranges, traverses the western foot hills of the northern most extent of the Tararua's. The road starts on the late pleistocene terraces of the Manawatu River, where the predominant landcover is predominantly high-producing exotic grassland, and climbs off the terrace into gullied hills made up of late pleistocene shoreline deposits, with overlying loess deposits (geological information sourced from [QMAP](https://www.gns.cri.nz/Home/Our-Science/Land-and-Marine-Geoscience/Regional-Geology/Geological-Maps/1-250-000-Geological-Map-of-New-Zealand-QMAP "1:250000 Geological Map of NZ"){:target="_blank"}
), where the grassland becomes more of a mix of indigenous forest patches, alongside blocks of exotic forest.

![Checklist site]({{site.url}}/images/IMG_8756.JPG "Checklist site")

This road seemed as good as any to collect checklists along, with the opportunity to look at a range of habitat types. We had looked at the locations recorded for the NZ Bird Atlas against the grid square the road was in ([BO75](https://ebird.org/atlasnz/block/blkBO75?atlasPeriod=EBIRD_ATL_NZ_2019&m=&rank=mrec "NZ Atlas Grid Square BO75"){:target="_blank"}), and most checklists had been centred on the Ashhurst Domain (which is to be expected given the diverse habitat types between the Ashhurst township, and the Pohangina River) or through the Manawatu Gorge (again, no surprise, given the number of walkers who use the tracks through the gorge). For us, it was an opportunity to add some additional habitat types to the atlas for this grid square.

We have three sites located along this road:
- The first is in the high-producing exotic grassland at the start of the road (just over the first bridge). 
- The second is about halfway up the road, underneath some mature pine trees alongside the road. 
- The third is toward the top of the road, in an area of recently harvest pine trees, that is adjacent to a gully with native bush on one side, and grazing land on the other.

These sites, in and of themselves, do not have the intrinsic values associated with the Ashhurst Domain or the Manawatu Gorge, but the they do fulfill the requirement of the atlas to collect data for a variety of habitats within each square. That said, the value of these sites grows as we go back to them and repeat the checklist collection through each season. Our checkist collection dates, so far, for this road have been:
- 11 Aug 2019
- 10 Nov 2019
- 12 Jan 2020

Given our novice skill level, we do not think we have identified all birds present (also noting that we are only spending about 30-40 minutes observing across the three sites). It is in going back time after time, and as we pick up ID skills along the way, that we hope to build a good summary of species present in this small patch of the Manawatu, and that this can benefit the NZ Atlas in the long-run.

Finally, here are the birds we have seen across all these sites so far:

```r
# Finding the Forest Hill Road sites in my downloaded checklist data 
forest_hill_road <- chklst[grepl("Forest Hill",chklst$Location),]

# Getting a list of species identified to date
unique(forest_hill_road$Common.Name)
 [1] "Paradise Shelduck"    "Mallard"              "New Zealand Pigeon"  
 [4] "Pukeko"               "Spur-winged Plover"   "Australasian Harrier"
 [7] "Sacred Kingfisher"    "Eastern Rosella"      "Tui"                 
[10] "Bellbird"             "Grey Warbler"         "Australian Magpie"   
[13] "New Zealand Fantail"  "Skylark"              "Welcome Swallow"     
[16] "Silvereye"            "European Starling"    "Song Thrush"         
[19] "Eurasian Blackbird"   "Dunnock"              "House Sparrow"       
[22] "Chaffinch"            "European Greenfinch"  "European Goldfinch"  
[25] "passerine sp."        "White-faced Heron"

# ... or alphabetically
unique(forest_hill_road$Common.Name)[order(unique(forest_hill_road$Common.Name))]
 [1] "Australasian Harrier" "Australian Magpie"    "Bellbird"            
 [4] "Chaffinch"            "Dunnock"              "Eastern Rosella"     
 [7] "Eurasian Blackbird"   "European Goldfinch"   "European Greenfinch" 
[10] "European Starling"    "Grey Warbler"         "House Sparrow"       
[13] "Mallard"              "New Zealand Fantail"  "New Zealand Pigeon"  
[16] "Paradise Shelduck"    "passerine sp."        "Pukeko"              
[19] "Sacred Kingfisher"    "Silvereye"            "Skylark"             
[22] "Song Thrush"          "Spur-winged Plover"   "Tui"                 
[25] "Welcome Swallow"      "White-faced Heron"   
> 

```

This last step is an example of being able to look at the data I want to, without resort to the eBird website. 

I view the NZ Atlas project as data collection exercise, using a really well designed app, and a website that provides lots a data visualistions, along with data summaries. There are limitations to the app and the website, and this is why I started this blog to start to highlight the additional things that can be done to help the Atlas project move forward.


