## README

This project aims to build a master dataset of verified burial mounds with coordinates, TRAP and TopoID, dimensions, landuse and conservation attributes. We have 4 datasets from 4 campaigns of fieldwalking:

- 2009 (20 ATTRIBUTES) field survey and remote sensing verification, initial season, data model not complete hence some missing attributes, 
- 2010 (58 ATTRIBUTES)legacy data verification on the basis of historical topographic maps, data model also not fully sorted,plus robbery of office leads to the loss of image data for one team's week of work, LU data not transcribed, many categories missing, additional discrepancy arises from asynchronous fixing by two different teams. Requires weeks of streamlining in OR, GoogleEarthPro, ArcGIS, and old PDfs of field records, images and spreadsheets to triangulate closer to the past reality.
- 2017 (46 ATTRIBUTES)legacy data verification with a mature data model, digital FAIMS output, 
- 2018 (58 ATTRIBUTES) repeat of 2017 in a different region (Bolyarovo) with minor tweaks to data model, but very active body of participants who added annotations everywhere.
- 2022 (58 ATTRIBUTES) repeat of 2017 in yet another region (Elhovo) with the same data model.

Difference in attributes stems from notes attached to individual fields in later seasons. 

## Streamlining

As in any archaeological fieldwork, the data realities and accompanying data models are emergent, and as such they evolve through time. I therefore encountered inconsistency from one dataset to another in time, with the initial datasets sufferring from more ommissions and errors, than latter ones where stricter controls and validation were implemented directly in the field. 

- 2009 survey dataset had undergone cleaning for the initial TRAP publication and my own dissertation and only required a couple hours to split up LU into _Around and _Top and verify dimensions and spotcheck factual correctness to be ready for a merge-in
- 2009 RS dataset required the same as 2009 survey
- 2010 was severely impaired by missing and divergent data (2016 adela version differed from 2018 bara version, with however held the promise of fuller attributes and GIS information). In order to verify and fill in information, I had to refine each version, reconcile discrepancies and duplicates and fill LU and dimensions in from GEPro before I could merge these two versions and gauge their utility. I used RS_Temporal data (completely different source) to verify landuse data, but it was not all that useful in the end. This dataset took in excess of 60 hours to reconcile
- 2017 took 2 hours to clean up
- 2018 took 2 hours to clean up (will need a bit more to bring into sync with all previous ones)
- 2022 took 8 hours to clean up due to relatively infrequent in-field checks (no Bara verifying the data daily) and the discovery of attribute issues (serendipity instead of survey as source, etc.) and the need to aggregate all five seasons and deal with attribute and spatial attributes.

It is perhaps useful to categorize the main issues that archaeologists seeking to reuse data encounter. 

### Within dataset issues:
Within each dataset there are quirks such a typographic and factual errors, such as duplicate IDs or erroneous IDs, or dot-separated decimals, misspellings, or multivalued attributes that prevent merging and analysis in R. These I have strived to reconcile in OpenRefine. Missing data were filled in where possible by revisiting field records, field photos, and by using temporal slider in satellite imagery viewer GoogleEarthPro. Furthermore, records were randomly spotchecked against scanned paper data to verify factual accuracy. To summarize the issues were divided between the technical inconsistencies due to lack of validation (fixable with computer-assistance of OR) and factual errors which required manual review.
Datasets were cleaned of the issues within before merging.

### Between dataset issues:
Once each dataset was reasonably consistent, the merging could commence. Upon mergin, I discovered that despite cleaning, vocabularies slightly differed among datasets (spelling, capitalisation, etc.) from one year to another even in established categories such as Landuse, and more under attributes such as CRM/Conservation. Column names also varied. This was tractable with grepl and attention. 
Divergence in the semantic model behind attributes was not tractable computationally but required reinterpretation: e.g. the application of Landuse has also shifted from 2009, when only a single landuse category was used without closer specification , while in 2017 we differentiated between Landuse_Around, Landuse_Top to specify what landuse was meant.  Having written the guidelines in 2009, I know I intended landuse around the mound, but some teams recorded on mound surface judging from the comparison of landuse visible in field photos and the spreadsheets.

## Spatial data: 
I don't even wish to go here now yet. :)
There is a number of spatial duplicates and triplicates (see duplicate_final.txt) for mounds repeatedly visited in different season either on assignment to take better photos or by accidental overlap, addressed and written up in YambolMoundAnalysis > 00b_SpatialDeduplication script.

# How to use

1. For first-time users, start by running the script `source("scripts/05_MergeToMaster.R")` to create a master dataset from the 2009-2022 data above. 
2. Afterwards, depending on your needs, either enrich data spatial using

* `06_GetSpatial.R` which loads the point shapefiles for the mounds and
* `06a_SpatialMerge` and `06b_SpatialEnrichment` which respectively merge spatial data and attributes, and extract environmental data from rasters at points (via `raster::extract`)

3. Look at and develop some of the following studies, whether on landuse classificaiton assessment (effect of perspective and discrepancy between remote sensing and field evaluation) or size effect on looting or other vulnerability.

