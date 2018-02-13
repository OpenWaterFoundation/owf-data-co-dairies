# owf-data-co-dairies #

This repository contains the [Open Water Foundation (OWF)](http://openwaterfoundation.org) dataset for Colorado dairies.
This is a foundational dataset that provides unique identifiers and other data for dairies and the identifiers can be used to link other datasets, such as municipal water providers.
OWF has created and is maintaining this dataset to facilitate work on various data analysis and visualization projects in Colorado.  **Note that this work is initially focused on the South Platte Basin.  The dataset will continue to be filled in as time and resources permit.** 

The following sections provide a summary of the project repository:

* [Connections to the Colorado Water Plan and the Statewide Water Supply Initiative 2010](#connections-to-the-colorado-water-plan-and-the-statewide-water-supply-initiative-2010)
* [How to Use the Data](#how-to-use-the-data)
* [Repository Contents](#repository-contents)
* [Attribution](#attribution)
* [Data Workflow](#data-workflow)
* [Data Issues](#data-issues)
* [Additional Resources](#additional-resources)
* [Disclaimer](#disclaimer)
* [License](#license)
* [Contributing](#contributing)
* [Maintainers](#maintainers)
* [Contributors](#contributors)

## Connections to the Colorado Water Plan and the Statewide Water Supply Initiative 2010 ##
The [Colorado Water Plan (CWP)](https://www.colorado.gov/cowaterplan), completed in 2015, is "a roadmap that leads to a productive economy, vibrant and sustainable cities, productive agriculture, a strong environment and a robust recreation industry."  The plan lays the groundwork for the measurable outcomes, goals and actions required to ensure that the State's municipal, industrial, agricultural, environmental and recreational needs are preserved.  Self-supplied industrial (SSI) water needs are a component of municipal and industrial needs.  SSI demands include water use by self-supplied and municipal-provided industries.  Subsectors of SSI include large industries (beer brewing, manufacturing, mining and food processing), snowmaking, thermoelectric power generation at coal- and natural gas-fired facilities and energy development.  While not specifically mentioned in the CWP, the dairy industry should be considered a component of SSI water demands.
The [Statewide Water Supply Initiative 2010 (SWSI 2010)](http://cwcb.state.co.us/water-management/water-supply-planning/pages/swsi2010.aspx) provides an analysis of consumptive and nonconsumptive water needs, as well as projects and methods to meet those needs.  When describing municipal and industrial water use projections, the dairy industry is not mentioned.  This leaves a lot of room for improvement in understanding water use within the dairy industry.  Furthermore, while SSI demands are approximately only 10% of that demanded by municipal and industrial (M&I) demands in the South Platte basin, the demand is predicted to increase in the future (SWSI p.4-18).  The dairy industry is largely focused within the South Platte basin (see basic map [here](https://github.com/OpenWaterFoundation/owf-data-co-dairies/blob/master/data/Colorado-Dairies.geojson)); therefore it may be of particular importance in understanding future water needs for the basin.


## How to Use the Data ##

The Colorado Dairies dataset intends to provide a complete statewide list of dairies assembled from multiple sources.  There are identifiers for each dairy and the dataset allows cross-referencing the identifiers so that other datasets can be joined.  For example, the [Colorado Municipal Water Providers dataset](https://www.github.com/OpenWaterFoundation/owf-data-co-municipal-water-providers) uses municipal water provider identifiers and can be used to link to this dataset to provide information about water use.  
Other potential uses of this dataset include the following.  Note that this largely requires the use of additional data that are not currently in the dataset.  Data issues are discussed in the Data Issues section.
* Creating a point map of dairies to indicate where the dairy industry is concentrated.
* Creating a network graph of dairies and their water providers.  This can serve to help understand where dairies get their water and which water providers are the largest suppliers of water to dairies.
* Creating a network graph of dairies and water sources (streams).  This can serve to help understand the rivers and creeks that supply water to dairies.
* Creating county choropleth maps that indicate annual dairy products (milk, cheese, butter, etc.) production totals or sales totals.  This can serve to illustrate how the dairy industry contributes to the economies of each county and the South Platte basin as a whole.

The Excel and csv files can be used as tabular datasets as is, to create filtered lists or to link to other datasets.  Data-processing software such as TSTool can be used to link this dataset to other datasets.

The format and contents of the dataset will change over time.  It is recommended to save a copy of the dataset.


## Repository Contents ##

The repository contains the following:

```text
analysis/                                   TSTool software command files to process data into useful forms.
  Process-xlsx-to-csv.TSTool                TSTool command file that processes the core dataset from .xlsx to .csv.
  Process-xlsx-to-geojson.TSTool            TSTool command file that processes the core dataset from .xlsx to .geojson.  
data-orig/                                  Folder containing original data files downloaded from agency websites.
  Dairy-BusinessEntity-IDs.csv              The data file containing original data downloaded from the Colorado Information Marketplace containing Business Entity IDs that has been reformatted in TSTool into a more usable form
data-orig-process/                          Folder containing files, such as TSTool command files, for processing original data into usable formats
  Dairy-BusinessIDs-original.csv            The data file containing original data downloaded from the Colorado Information Marketplace containing Business Entity IDs for entities registered with the Colorado Department of State.
  Process-original-data-to-csv.TSTool       TSTool command file that processes data either directly from websites or data files manually downloaded from websites to be incorporated into the main dataset
data/                                       Folder containing data files.
  Colorado-Dairies.csv                      The Excel file contents from the Dairy worksheet converted to a csv file, useful for automated processing.
  Colorado-Dairies.geojson                  Spatial data file of the locations of dairies.
  Colorado-Dairies.xlsx                     Simple Excel file containing core data.
doc/
  ?                                         Additional documentation for the dataset.
.gitattributes                              Git configuration file indicate repository configuration, in particular handling of line-ending and binary files.
.gitignore                                  Git configuration file to ignore files that should not be committed to the repository.
README.md                                   Explanation of repository contents, data files and sources and TSTool command files used to process the core data into other products.
```

### Colorado-Dairies.xlsx Contents ###

The core Excel workbook that serves as the master data contains the following data columns within the **Dairy** worksheet.

* **DairyName** - name of the dairy
* **BusinessEntity_ID** - state-assigned identification number for a business entity, from the Colorado Information Marketplace's [Business Entities in Colorado](https://data.colorado.gov/Business/Business-Entities-in-Colorado/4ykn-tg5h/data) dataset, to link state datasets
* **BusinessEntity_ID_Flag** - data status of BusinessEntity_ID values; see more detail below
* **Physical_Address** - street address of the physical location of the dairy; from the "Business Entities in Colorado" dataset
* **Municipality** - municipality in which the dairy is located; from the "Business Entities in Colorado" dataset
* **State** - state in which the dairy is located
* **ZipCode** - zip code in which the dairry is located; from the "Business Entities in Colorado" dataset
* **Municipality_DOLA_LG_ID** - 5-digit identifier used by Colorado's [Department of Local Affairs (DOLA)](https://dola.colorado.gov/lgis/municipalities.jsf) for the municipality in which the dairy is located, to link DOLA datasets
* **Municipality_DOLA_LG_ID_Flag** - data status of Municipality_DOLA_LG_ID values; see more detail below
* **Municipal_WaterProvider_OWF_ID** - unique text identifier of the municipal water provider that provides water to the dairy; created by OWF
* **Municipal_WaterProvider_OWF_ID_Flag** - data status of Municipal_WaterProvider_OWF_ID values; see more detail below
* **Latitude** - latitude of dairy's point location in decimal degrees
* **Longitude** - longitude of dairy's point location in decimal degrees
* **Lat_Long_Flag** - indication of how latitude and longitude were determined; g1 = coordinates are based on the physical address of the dairy
* **WaterSource_GNIS_Name** - [Geographic Names Information System](https://geonames.usgs.gov/apex/f?p=138:1:9185633219989) name of the stream from which the dairy receives water, to link federal and state datasets
* **WaterSource_GNIS_Name_Flag** - data status of WaterSource_GNIS_Name values; see more detail below
* **WaterSource_GNIS_ID** - [Geographic Names Information System](https://geonames.usgs.gov/apex/f?p=138:1:9185633219989) identifier of the stream from which the dairy receives water, to link federal and state datasets
* **WaterSource_GNIS_ID_Flag** - data status of WaterSource_GNIS_ID values; see more detail below
* **Formation_Date** - date the dairy began operation or was incorporated
* **Formation_Date_Flag** - data status of Formation_Date values; see more detail below
* **Dissolution_Date** - date the dairy ceased operations
* **Dissolution_Date_Flag** - data status of Dissolution_Date values; see more detail below
* **Website** - website URL of the dairy
* **Website_Flag** - data status of Website values; see more detail below


#### Data Flags ####
For many data columns, a second column of the same name with the word "_Flag" added to the column name is present.  These columns are an indication of data status as it relates to missing data.  The following conventions are used:
* G = Value is a known/good value.  
* g = Value is an estimated (but good) value.  The associated cell is also highlighted in yellow.
* N = Value is not applicable for the municipality and a blank cell is expected.
* I = Incomplete values; cell has been populated but may not yet contain all values or may need to be further verified
* M = Value is known to be missing in original source and therefore a blank cell indicates that a value cannot be provided.
* m = Value is estimated to be missing.  The associated cell is also highlighted in gray.
* z = Value is unable to be confirmed.  A value is possible but cannot be confirmed one way or the other.  The associated cell is also highlighted in orange.
* x = OWF has not made an attempt to populate the cell at this time.  The value is missing because OWF has not attempted to find the value.  The associated cell is also highlighted in black.

*Note that colors are visible only in xlsx files and not csv files.*

Single-character flags may also be followed with a number, as in g1.  These flags are specific to certain columns and are detailed above in the descriptions of the data columns.  

Column names are taken from original sources if possible.  For clarity and attribution, agency abbreviations may be added to the original column name.  Column name length is not restricted, therefore, some data representations such as Esri shapefiles may contain truncated column names.  In such cases, alternative formats such as GeoJSON are recommended.

Descriptions of identifiers are also provided in the **Notes** worksheet within the workbook.  This worksheet also details how the original data were downloaded and where to find those files.


Other worksheets within the workbook contain the following:

* **ChangeLog** worksheet indicates any changes made to the dataset, the date they occurred and who made the changes.

* **Metadata_Dairy** worksheet serves as the metadata for data columns in the **Dairy** worksheet.


### Colorado-Dairies.csv Contents ###
This file is the **Dairy** worksheet saved in csv format.  Warning:  if this file is opened directly in Excel, IDs that contain leading zeroes will not show those zeroes.  Instead, import the file into a blank Excel file by selecting Data/Get External Data/From Text.


### Colorado-Dairies.geojson Contents ###
This file is the **Dairy** worksheet saved in GeoJSON format.  This file should be viewable as a map in the GitHub repository.  It can also be used in GIS and mapping applications.


## Attribution ##

The data sources for this dataset are listed below.

* This dataset was first created by downloading the [Business Entities in Colorado](https://data.colorado.gov/Business/Business-Entities-in-Colorado/4ykn-tg5h/data) dataset from the Colorado Information Marketplace.  Data were filtered to only include entities with the word "dairy" in the entity name.  The CSV file was then opened in TSTool where further processing of the data occurred.  If a dairy is no longer in operation, then the words "Dissolved" with a date are also listed in the dairy's name.  The dissolution date was split from the dairy's name and used to create the Dissolution_Date data column.  This dataset is the source for the BusinessEntity_ID, DairyName, Physical_Address, Municipality, State, ZipCode, Formation_Date and Dissolution_Date data columns. The BusinessEntity_ID is a state-assigned identification number.
* The Colorado Department of Local Affairs (DOLA)'s [Local Government Information System](https://dola.colorado.gov/lgis/counties.jsf) uses a local government ID (LG ID) for municipalities.  OWF is using DOLA_LG_ID instead of LG ID to add more description to the identifier.  Municipality_DOLA_LG_IDs come from the [Colorado Municipalities](https://github.com/OpenWaterFoundation/owf-data-co-municipalities) dataset created by OWF.
* OWF created municipal water provider IDs in its [Colorado Municipal Water Providers](https://github.com/OpenWaterFoundation/owf-data-co-municipal-water-providers) dataset, to be able to link water provider data, such as water use.
* Each dairy has a physical address listed in the dataset, but not latitude/longitude coordinates.  To obtain coordinate data, the dataset was opened in [Google Sheets](https://www.google.com/sheets/about/).  An add-on named Geocode by Awesome Table can take a physical address and convert it to coordinates, as long as the street address, city, state and zip code are provided.
* The U.S. Geological Survey (USGS)'s [Geographic Names Information System (GNIS)](https://geonames.usgs.gov/apex/f?p=138:1:9185633219989) is the Federal and national standard for geographic nomenclature.  The USGS developed the GNIS in support of the U.S. Board on Geographic Names as the official repository of domestic geographic names data.  Most streams in Colorado have a GNIS Name and GNIS ID.  GNIS names and identifiers are also used within the [Source Water Route Framework](http://cdss.state.co.us/GIS/Pages/AllGISData.aspx), a spatial data layer that contains measured, named streams from the National Hydrography Dataset.
* At this time, website URLs are found by manually searching for dairy websites.


## Data Workflow ##
The general workflow is as follows once the basic dataset is created:
1. Data flags are created for many of the data columns that indicate data status as described above.
2. The data are formatted as a table to allow for data filtering.
3. The dataset is saved in .xlsx format.
4. The xlsx-formatted file is opened in TSTool and a short command file [(Process-xlsx-to-csv.TSTool)](https://github.com/OpenWaterFoundation/owf-data-co-dairies/blob/master/analysis/Process-xlsx-to-csv.TSTool) converts the dataset into CSV format, to be used in additional data-processing steps.
5. The xlsx-formatted file is opened in TSTool and a short command file [(Process-xlsx-to-geojson.TSTool)](https://github.com/OpenWaterFoundation/owf-data-co-dairies/blob/master/analysis/Process-xlsx-to-geojson.TSTool) converts the dataset into GeoJSON format.  This step is optional and applicable for datasets in which a map will be created or if further processing will occur in GIS application such as QGIS.


## Data Issues ##
In the development of this dataset, OWF found it difficult to obtain information about the dairy industry.  Indeed, there does not appear to be an industry organization that provides a list of its members.  For example, the [Western Dairy Association](https://westerndairyassociation.org/), headquartered in Thornton, Colorado, does not list any of its members, other than to say that it serves over 120 dairy farms in Colorado, Montana and Wyoming.
To advance the utility of this dataset, the following data sources may need to be considered:
* To obtain additional data about dairy production, it may be necessary to look at the county level.  The USDA's [National Agricultural Statistics Service (NASS)](https://www.nass.usda.gov/index.php) is a federal agency tasked with surveying every aspect of agriculture.  Examples of some of the data it collects include:  production and supplies of food and fiber, prices paid and received by farmers, farm labor and wages and chemical use.  It also conducts the Census of Agriculture every five years, which provides detailed agricultural data for every county in the U.S.  NASS's [Colorado Field Office](https://www.nass.usda.gov/Statistics_by_State/Colorado/) has a searchable database, [Quick Stats](https://quickstats.nass.usda.gov/), that provides access to county-level data for commodities like milk sales.
* For water use data, again it may be necessary to look at the county level.  The U.S. Geological Survey (USGS)'s National Water Information System has [water use](https://waterdata.usgs.gov/nwis/wu) data at the county level for each state.  Water use is classified into several categories, one of which is livestock.  Livestock water use is associated with livestock watering, feedlots, dairy operations and other on-farm needs.  Livestock includes dairy and beef cattle, as well as sheep, goats, hogs, horses and poultry.  It would not be possible to tease apart dairy water use from overall livestock water use, but this data could potentially be used in some fashion.  
* Some municipal water providers may have information about the dairy industry in their water conservation/efficiency plans.  For example, North Weld County Water District's [Water Conservation Plan](http://nwcwd.org/wp-content/uploads/2017/12/nwcwd_wcp_public.pdf) separates dairies from other commercial water uses because the dairy industry is a large source of water use in the district.  Water efficiency plans may yet be another source of data, albeit at the water provider level.

	
## Additional Resources ##
* [Western Dairy Association](https://westerndairyassociation.org/)
* [USDA's National Agricultural Statistics Service (NASS) Colorado Field Office](https://www.nass.usda.gov/Statistics_by_State/Colorado/index.php)

## Disclaimer ##

OWF has created this initial dataset of Colorado dairies.  OWF will attempt to fill data gaps as the dataset is used for analysis and funding allows for more data review.  OWF provides no guarantee as to the accuracy of the data.  **Use this dataset at your own risk.**  OWF welcomes feedback to improve the dataset.

## License ##

The license is being determined.  All the data are public so there are not really any restrictions on use.

## Contributing ##

The Open Water Foundation created this dataset as a general resource with simple analyses.
If you use the dataset and have comments, please contact the maintainers and/or use the GitHub issues to provide feedback.

## Maintainers ##

Kristin Swaim (@kswaim, kristin.swaim@openwaterfoundation.org) is the primary maintainer at the Open Water Foundation.

Steve Malers (@smalers, steve.malers@openwaterfoundation.org) is the secondary contact.

## Contributors ##

None yet, other than OWF staff.
