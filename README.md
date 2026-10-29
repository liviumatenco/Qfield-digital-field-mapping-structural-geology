# Manual for building a digital field mapping system for structural geology using Qfield for QGIS  
## Used by Utrecht University BSc GEO3-1307 Structural Geology and MSc GEO4-1430 Fieldwork Research Instruction Geology courses  
**Author:** Liviu Matenco  
**Institution:** Utrecht University  
**Version:** 2.0  
**License:** MIT License   
  
## Requirements:   
### **QGIS for any computer:** to define the digital field mapping project and post-fieldwork processing, https://qgis.org  
### **Qfield for mobile devices/smartphones with GPS hardware incorporated:** for fieldwork mapping, https://qfield.org  
  
### The example solution `UU_GEO_fieldwork` is a students fieldwork area in SE Spain.   
### The dataset provided is not real, has been modified for educational purposes.
Note: The project and layers describe the CRS projection Worldwide system: Pseudo-Mercator / Ellipsoid Wold Geodetic System 1984 (WGS84), EPSG: 3857, which is less (1-2m) precise. If you need a better resolution, use your local projection system, but define the same CRS for the project and for the enclosed layers.
  
---  
## What's New in Version 2.0
* **Dynamic Categories:** Replaced static characterisation of `Types` to a dynamic one
* **Generations of structures:** Assignment of levels of the same type (i.e . generations) in a `Generation` field.
* **Replaced external png symbols:** Internal QGIS/Qfield Simple symbols are now used.
* **Support for the Qfield Structural Geology Calculator, v 2.0:** Dynamic assignment of the `Types` field. See https://github.com/liviumatenco/Qfield-Structural-Geology-Calculator.
    
# Workflow in QGIS  
### QGIS has different setups in various versions. The workflow is build in QGIS 4.2.1 for MacOS (27, Golden Gate). You may need to update the guide to your setup.  
  
> First install the Qfield Sync plugin for QGIS:  
`Plugins` -> `Manage and install plugins` -> `Qfield Sync` -> `Install`  
## Step 1: Project Definition.  
Create a folder called `UU_GEO_fieldwork` with a sub-folder called `photos`, another sub-folder called `maps`  
and another sub-folder called `fieldwork`.   
Move the `symbols` folder from the `supplementary files` folder to the `UU_GEO_fieldwork` folder.  
Open QGIS  
Project -> Properties  
---General  
------Save paths  
---------// The path is set to relative starting with the `UU_GEO_fieldwork` folder  
---------Relative -> Apply  
---CRS  
------Filter  
---------// Worldwide system: Pseudo-Mercator / Ellipsoid Wold Geodetic System 1984 (WGS84), EPSG: 3857  
---------3857 -> Select -> Apply  
---Qfield  
------Attachments and folders  
---------// Limiting the resolution of pictures to avoid creating projects too large in size  
---------Maximum pixels  
------------2400 px  
---Save and close Project properties (OK)  
Project -> Save as ...  
---Save in the root folder UU_GEO_fieldwork with the name `UU_GEO_fieldwork.qgz`  
## Step 2: Create a geopackage file containing multiple layers/tables  
### Create the first file  
Layer -> Create layer -> New GeoPackage Layer ...  
---Filename  
------Save in sub-folder fieldwork with the name `fieldwork.gkpg` -> Save  
---Table name  
------// Layer containing your stops  
------locations  
---Geometry type  
------Point  
---Include Z dimension  
------yes  
---Include M Value  
------yes  
---EPSG  
------Universal Transverse Mercator (UTM) Z0ne 30N / Ellipsoid European Datum 1950 (ED50), EPSG: 3857  
---// Define de follwowing structure, case sensitive  
---Field List, all types are Text (string)  
------uuid  
------name  
------description  
------color  
------rock type  
------rock  
------Team  
------timestamp  
------latlong(WGS84)  
------xy-UTM30(ED50)  
------Altitude  
------Scope  
---Save/OK  
### Continue creating other layers/tables in the same geopackage file  
Browser panel -> Project home -> fieldwork -> Right click `fieldwork.gkpg` -> New Table ...  
---Name  
------// Layer containing your structural measurements  
------measurements  
---Geometry type  
------Point  
---Include Z dimension  
------yes  
---Include M Value  
------yes  
---EPSG  
------// Worldwide system: Pseudo-Mercator / Ellipsoid Wold Geodetic System 1984 (WGS84), EPSG: 3857  
      3857  
---// Define de follwowing structure, case sensitive  
---Add Field, all types are Text (string)  
------Type---------type Text (string)  
------Generation--------type Integer (32 bit)
------Dip azimuth------type Integer (32 bit)  
------Dip---------type Integer (32 bit)  
------Trend---------type Integer (32 bit)  
------Plunge------type Integer (32 bit)  
------Top to------type Integer (32 bit)  
------Attachment------type Text (string)  
------Team---------type Text (string)  
------timestamp------type Text (string)  
------latlong(WGS84)---type Text (string)  
------xy-UTM30(ED50)---type Text (string)  
------Altitude------type Text (string)  
------Scope---------type Text (string)  
---Save/OK  
### Continue creating other layers/tables in the same geopackage file   
Browser panel -> Project home -> fieldwork -> Right click fieldwork.gkpg -> New Table ...  
---Name  
------// Layer containing your locations photos (multiple possible in each stop)  
------photos  
---Geometry type  
------No geometry  
---// Define de follwowing structure, case sensitive  
---Add Field, all types are Text (string)  
------photo_uuid  
------photo  
------direction  
------timestamp  
------latlong(WGS84)  
------Altitude  
------filename  
------Scope  
---Save/OK  
### Continue creating other layers/tables in the same geopackage file  
Browser panel -> Project home -> fieldwork -> Right click fieldwork.gkpg -> New Table ...  
---Name  
------// Layer containing your interpretations of lithological, stratigraphic, structural or tectonic contacts  
------lines  
---Geometry type  
------LineString  
---Include Z dimension  
------yes  
---Include M Value  
------yes  
---EPSG  
------// Worldwide system: Pseudo-Mercator / Ellipsoid Wold Geodetic System 1984 (WGS84), EPSG: 3857  
      3857  
---// Define de follwowing structure, case sensitive  
---Add Field, all types are Text (string)  
------type  
------description  
------color  
---Save/OK  
### Continue creating other layers/tables in the same geopackage  
Browser panel -> Project home -> fieldwork -> Right click fieldwork.gkpg -> New Table ...  
---Name  
------// Layer containing your interpretations in areal extent (usually stratigraphic units)  
------polygons  
---Geometry type  
------MultiPolygons  
---Include Z dimension  
------yes  
---Include M Value  
------yes  
---EPSG  
------// Worldwide system: Pseudo-Mercator / Ellipsoid Wold Geodetic System 1984 (WGS84), EPSG: 3857  
      3857  
---// Define de follwowing structure, case sensitive  
---Add Field, all types are Text (string)  
------name  
------description  
------color  
---Save/OK  
### Define a table with lithological types to be selected, separated by categories  
// ---you can use a simple definition available in the supplementary file `lithology_value_relation.csv`  
Layer -> Add Layer -> Add delimited text layer  
---Open lithology_value_relation.csv in supplementary file  
---File format  
------CSV (comma separated values)  
---First record has field names  
------yes  
---Geometry Definition  
------No geometry (attribute table only)  
---Add -> Close  
**Drag and drop lithology_value_relation in the layers panel to fieldwork.gkpg in the Browser panel -> OK**  
**Remove lithology_value_relation from the Layers panel**  
  
## Step 3: create online and offline maps, organize project  
### Online Open Street Map  
// Online map, add OSM standard plugin and map (Whole World)  
Plugins -> Manage and Install Plugins -> QuickMapServices -> Install plugin  
Web -> Quick Map Services -> OpenStreetMap -> OSM Standard  
### Offline Spain topographic and geological maps  
Zoom out and in to your interesting area  
// For Spain you can install ArcGIS REST Servers for both geological and topographic maps  
// ---See https://www.igme.es and https://centrodedescargas.cnig.es/CentroDescargas/btn  
// ---Use the 1:50.000 GEODE geological map and 1:2.000-25.0000 BTN topographic map  
// For efficiency I provide the current definition as two Layer Definition Files available  
// ---in the `supplementary files` folder for your area in SE Spain  
// ---1:50.000 GEODE map  
Layer -> Add from Layer Definition File  
---Select `GEODE 50K.qlr` -> Open  
// 1:2.000-25.000 topographic map  
Layer -> Add from Layer Definition File  
---Select `BTN_Spain_topo.qlr` -> Open  
### Create offline maps  
// Offline map can be created for any of the online maps above (or their combination)   
// ---by selecting/zooming in an area by using the Generate XYZ tiles (MBTiles) raster tool  
// ---You must be online for the instructions below (data transfer from ArcGIS servers)  
Processing -> Toolbox -> Search -> Generate XYZ tiles (MBTiles)  
---Extent  
------Either the current zoom (Draw from Map Canvas) of by defining a polygon  
---Minimum zoom  
------6  
---Maximum zoom  
------15 for geological maps, 16 for topographic maps  
---Tile format  
------JPG (smaller files)  
---Output  
------Save to file ...  
---------Save to GEODE, BTN or OSM in the maps sub-folder  
---Run  
### Organize your project  
// When you added the first layer QGIS has changed the projection system.  
// Put it back to EPSG 3857  
Project -> Properties  
---CRS  
---------3857 -> Select -> Apply  
// Add all fieldwork.gkpg layers to the project by double-clicking the fieldwork.gkpg layers in the Browser panel  
//---Note that you already have some of them in the Layers panel  
// Organize by defining groups (Layers panel - Add group) and moving/adding layers to these groups  
// Create the following structure  
photos  
lithology_value_relation  
fieldwork (group)  
---locations  
---measurements  
---lines  
---polygons  
maps (group)  
---offline (sub-group)  
------// Add your own  
---online (sub-group)  
------BTN_Spain_topo  
------GEODE 50K  
------OSM Standard  
## Step 4: Linking the multiple photos in the photos layer with one location in the locations layer  
Project -> Properties -> Relations -> Add relation  
---Name---Attachment 
---Referenced Layer---locations 
---Referenced Field(s)---uuid 
---Referencing Layer---photos 
---Referencing Field(s)---photo_uuid 
---Id---attachments_relationship 
---Strength---Association  
OK -> Save project  
  
## Step 5: Defining the properties of layers and every field in various layers.  
// Double click on every layer/table  
fieldwork (group)  
---locations  
------Symbiology  
---------Rule based  
------------// Below is just one definition for all points with a second phto symbol when points contain photos  
------------// ---You can have different symbols based on what you want, for instance based on the Scope field.  
------------// ---You can define each symbol by filtering in various fields (e.g., "Scope" = 'fieldwork')  
------------// Press plus to add rules  
------------Points (Simple marker, select the default circle)   
---------------(no filter)  
---------------Size 2.4  
---------------Stroke width 0.6  
---------------Fill color  
------------------blue, 50% transparent  
------------------function -> `with_variable('c',if("color" is not null and "color" != '', "color", @value),set_color_part(@c, 'alpha', 128))`  
---------------Stroke color  
------------------blue  
------------------function -> `if("color" is not null and "color" != '', "color", @value)`  
------------Photos (Simple marker, select the default square)
---------------Filter, function -> `array_contains(aggregate('photos','array_agg',"photo_uuid"),"uuid")`  
---------------Size 4  
---------------Stroke width 0.6  
---------------Fill color  
------------------blue, 100% transparent  
---------------Stroke color  
------------------blue  
------------------function -> `if("color" is not null and "color" != '', "color", @value)`  
------------Symbol levels  
---------------Points 1  
---------------Photos 0  
------Labels  
---------// Bold 8 with a white buffer, next to the point, visible below 1:100.000 scale  
---------Single Labels  
---------Value  
------------name  
---------Text  
------------Style  
---------------bold  
------------Size  
---------------8  
---------Buffer  
------------Draw text buffer  
---------------yes   
---------Placement  
------------Mode  
---------------Around Point  
---------Rendering  
------------Scale dependent visibility: Yes  
---------------1:100000  
---------------0  
      Attribute form, Type Drag and Drop Designer  
---------Remove fid and uuid from Drag and Drop Designer, but set the following property for uuid  
------------uuid  
---------------Widget: hidden  
---------------Default value:  
------------------uuid()  
---------Add new tab called 'Data'. Inside put the following fields:---  
---------------name  
---------------Editable: yes  
---------------Widget: Text Edit  
------------description  
---------------Editable: yes  
---------------Widget: Text Edit  
------------color  
---------------Editable: yes  
---------------Widget: Value Map  
------------------// You can make your own definition. A few colors already defined  
------------------Load Data from CSV file -> supplementary file file `colors.csv`  
---------------Default value  
------------------// Default is blue, change as you wish  
------------------function -> `'#377EB8'`  
---------------When adding features: Reuse Last Entered Value  
---------Add new tab called 'Rocks'. Inside put the following fields:  
------------rock type  
---------------Editable: yes  
---------------Widget: Value Map  
------------------Load Data from CSV file -> supplementary file file `rock type.csv`  
---------------When adding features: Reuse Last Entered Value  
------------rock  
---------------Editable: yes  
---------------Widget: Value Relation  
------------------// few simple geological rock lithologies  
------------------Layer: lithology_value_relation  
------------------Key column: Value  
------------------Value column: Description  
---------------Order by  
------------------Field: fid  
---------------Filter expression  
------------------function -> `"Category" = current_value('rock type')`  
---------------When adding features: Reuse Last Entered Value  
---------Add new tab called 'Photos'. Inside put the following fields:---  
------------Attachment  
---------Add new tab called 'Information'. Inside put the following fields:---  
------------Team/Geologist  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------When adding features: Reuse Last Entered Value  
------------timestamp  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Takes the system date and time  
------------------function -> `coalesce(format_date(now(),'yyyy-MM-dd @ HH:mm'),'N/A')`  
------------latlong(WGS84)  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Calculate the coordinates from the position of point in the map, update when moved  
------------------function -> `coalesce(concat(round(y(transform($geometry,@layer_crs,'EPSG:4326')),6),',',round(x(transform($geometry,@layer_crs,'EPSG:4326')),6)),'N/A')`  
---------------Apply default value on update: Yes  
------------xy-UTM30(ED50)  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Calculate the coordinates from the position of point in the map, update when moved  
------------------function -> `coalesce(concat(round(x(transform($geometry,@layer_crs,'EPSG:23030')),2),',',round(y(transform($geometry,@layer_crs,'EPSG:23030')),2)),'N/A')`  
---------------Apply default value on update: Yes  
------------Altitude  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value  
------------------// Calculate the Altitude form the internal GPS sensor, do NOT update when moved  
------------------function ->  
------------------CASE  
                     WHEN @gnss_coordinate IS NULL THEN NULL  
                     ELSE replace(format_number(z(@gnss_coordinate), 3),',','.')  
                  END  
------------Scope  
---------------Widget: Value Map  
------------------Load Data from CSV file -> supplementary file file `scope.csv`  
---------------Default value:   
-------------------> function `'fieldwork'`  
---------------When adding features: Reuse Last Entered Value  
------Display  
---------Display   
------------Display Name  
------------function -> `COALESCE( "name", '<NULL>' )`  
---------Sort Order  
------------fid  
   measurements  
------// Because creating individual sysmbols is labour intensive, you may skip this entire section by directly 
------// ---loading the style of the entire layer in the file `measurements_style.qml` available in the supplementary files
------Symbiology  
---------// These symbols have to be defined manual. Symbols are custom defined by you  
---------//---In all symbols the azimuth of dip has to point north (azimuth north) in their definition  
---------//---You can also use the symbols available in the `symbols` sub-folder  
---------// Insert the first two columns in the table below in the list  
---------//---and double click the symbol when the line is defined  
---------Categorized, Value: Type, add by pressing the plus symbol,   
---------Symbol layer type: Simple symbol, and create your own symbols for: 
------------// plane-line, plane-line-sense require two symbols, one for the plane and one for the line
------------// fault and shearzone require two symbols, one for the plane and one for the line with an arrow
------------// ---press plus in the symbol selector to add one more symbol  
------------plane---plane
------------line---line
------------plane-line---plane-line
------------line-sense---line-sense
------------plane-line-sense---plane-line-sense
------------bedding---bedding
------------foliation---foliation
------------fault---fault
------------shearzone---shearzone
------------fault plane---fault plane
------------fold---fold
------------lineation---lineation
------------stretching lineation---stretching lineation
------------slickenlines---slickenlines
------------fault gouge foliation---fault gouge foliation
------------Riedel shear---Riedel shear
---------// All symbols: Symbols are rotated based on theit type: planes with `Dip azimuth`, lines with `Trend` and sense with `Top to`  
---------Rotation:  
------------function -> `"Top to"`---  
---------// All symbols:  
---------Size of the longest  
------------8  
---------// You may want to make the sense of shear for faults with sense of shear smaller, like 6   
---------// ---and vertically align it at the bottom of the symbol to start from the fault  
------Labels  
---------// Make all labes bold 8 with a white buffer, next to the point, visible below 1:100.000 scale  
---------Rule-based Labeling  
---------// Press plus to define two rules (planes or lines)  
---------first rule  
------------Filter  
---------------function -> `COALESCE("Type", '') IN ('', 'bedding', 'fault', 'shearzone', 'foliation', 'fault plane', 'fault gouge foliation', 'Riedel shear', 'plane', 'plane-line', 'plane-line-sense')`  
------------Value  
---------------Dip  
---------second rule  
------------Filter  
---------------function -> `COALESCE("Type", '') IN ('','line', 'line-sense', 'fold', 'lineation','stretching lineation', 'slickenlines')`  
------------Value  
---------------Plunge  
---------// For both rules  
---------Text  
------------Style  
---------------bold  
------------Size  
---------------8  
---------Buffer  
------------Draw text buffer  
---------------yes   
---------Placement  
------------Mode  
---------------Around Point  
---------Rendering  
------------Scale dependent visibility  
---------------1:100000  
---------------0  
      Attribute form, Type Drag and Drop Designer  
---------Remove fid from Drag and Drop Designer  
---------Create a new tab called 'Data' by pressing the plus button  
------------Label  
---------------Data  
------------Container type  
---------------Tab  
------------Number of columns  
---------------1  
---------// Inside put the following field (drag with your mouse)  
------------Type  
---------------Editable: yes  
---------------Widget: Value Map  
------------------Load Data from CSV file -> supplementary file file `type.csv`  
---------------Constraints  
------------------Not null  
------------------Enforce not null constraint  
---------------Generation  
------------------Editable: yes  
------------------Widget: Range  
---------------------Editable  
---------------------Minimum: 0  
---------------------Maximum: 9  
---------------------Step: 1  
------------------Allow null
---------------------Disabled
---------Create 3 (three) rows as show/hide placeholders for the data.  
------------Label  
---------------<leave empty>  
------------Container type  
---------------Row  
------------Within  
---------------Data  
-------------> press OK  
------------First row  
---------------Control visibility by expression  
---------------function -> `COALESCE("Type", '') IN ('', 'plane', 'plane-line', 'plane-line-sense', 'bedding', 'fault', 'shearzone', 'foliation', 'fault plane', 'fault gouge foliation', 'Riedel shear')`  
------------// Inside the first row put the following fields (drag with your mouse)  
---------------Dip azimuth  
------------------Editable: yes  
------------------Widget: Range  
---------------------Editable  
---------------------Minimum: 0  
---------------------Maximum: 360  
---------------------Step: 1  
------------------Allow null  
---------------Dip  
------------------Editable: yes  
------------------Widget: Range  
---------------------Editable  
---------------------Minimum: 0  
---------------------Maximum: 90  
---------------------Step: 1  
------------------Allow null  
------------Second row  
---------------Control visibility by expression  
---------------function -> `COALESCE("Type", '') IN ('', 'line', 'plane-line', 'line-sense', 'plane-line-sense', 'fault','shearzone','fold', 'lineation','stretching lineation','slickenlines', 'plane with line')`  
------------// Inside the second row put the following fields (drag with your mouse)  
---------------Trend  
------------------Editable: yes  
------------------Widget: Range  
---------------------Editable  
---------------------Minimum: 0  
---------------------Maximum: 360  
---------------------Step: 1  
------------------Allow null  
---------------Plunge  
------------------Editable: yes  
------------------Widget: Range  
---------------------Editable  
---------------------Minimum: 0  
---------------------Maximum: 90  
---------------------Step: 1  
------------------Allow null  
------------Third row  
---------------Control visibility by expression  
---------------function -> `COALESCE("Type", '') IN ('line-sense','plane-line-sense','fault','shearzone','stretching lineation','slickenlines')`  
------------// Inside the third row put the following field (drag with your mouse)  
---------------Top to  
------------------Editable: yes  
------------------Widget: Range  
---------------------Editable  
---------------------Minimum: 0  
---------------------Maximum: 360  
---------------------Step: 1  
------------------Allow null  
---------Add new tab called 'Photo'. Inside put the following fields:---  
------------// Just one photo possible in this layer  
------------// ---Stores the photos full path and name relative to the project base   
------------Attachment  
---------------Widget: Text (string)  
---------------Alias  
------------------photo (only one)  
---------------Editable: yes  
---------------Widget: Attachment  
---------------Store path as  
------------------Relative to project path  
---------------Storage mode  
------------------File Paths  
---------------Type  
------------------Image  
---------Add new tab called 'Information'. Inside put the following fields:---  
------------Team/Geologist  
---------------// Your name(s)  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Reuse Last Entered Value  
------------timestamp  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Takes the system date and time  
------------------function -> `coalesce(format_date(now(),'yyyy-MM-dd @ HH:mm'),'N/A')`  
------------latlong(WGS84)  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Calculate the coordinates from the position of point in the map, update when moved  
------------------function -> `coalesce(concat(round(y(transform($geometry,@layer_crs,'EPSG:4326')),6),',',round(x(transform($geometry,@layer_crs,'EPSG:4326')),6)),'N/A')`  
---------------Apply default value on update  
------------------Yes  
------------xy-UTM30(ED50)  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Calculate the coordinates from the position of point in the map, update when moved  
------------------function -> `coalesce(concat(round(x(transform($geometry,@layer_crs,'EPSG:23030')),2),',',round(y(transform($geometry,@layer_crs,'EPSG:23030')),2)),'N/A')`  
---------------Apply default value on update  
------------------Yes  
------------Altitude  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Calculate the Altitude form the internal GPS sensor, do NOT update when moved  
------------------function ->  
------------------CASE  
                     WHEN @gnss_coordinate IS NULL THEN NULL  
                     ELSE replace(format_number(z(@gnss_coordinate), 3),',','.')  
                  END  
------------Scope  
---------------Editable: no  
---------------Widget: Text Edit  
---------------Default value:  
------------------// The creator of this guide  
------------------function -> `'© Utrecht University'`  
---------------Apply default value on update  
------------------Yes  
------Display  
---------Display   
------------Display Name  
---------------function ->---`COALESCE( "Type", '<NULL>' )`  
------------Sort Order  
---------------fid  
------Qfield  
---------Attachment settings  
------------Attachment  
---------------// Saves the photo files in the correct sub-folder with timestamp as name  
---------------function -> `'photos/measurements_' || format_date(now(),'yyyyMMddhhmmsszzz') || '.{extension}'`  
   photos  
------Attribute form, Type Drag and Drop Designer  
---------Remove fid and photo_uuid from Drag and Drop Designer  
---------Add new tab called 'Photos'. Inside put the following fields:  
------------photo  
---------------// Stores the photos full path and name relative to the project base  
---------------Editable: yes  
---------------Widget: Attachment  
---------------Store path as  
------------------Relative to project path  
---------------Storage mode  
------------------File Paths  
---------------Type  
------------------Image  
------------direction  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Azimuth taken for the photo, taken from the internal compass  
------------------function -> `if(@gnss_orientation != 'nan', format_number(@gnss_orientation, 1) || ' °', 'N/A')`  
---------Add new tab called 'Information'. Inside put the following fields:  
------------timestamp  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Takes the system date and time  
------------------function -> `coalesce(format_date(now(),'yyyy-MM-dd @ HH:mm'),'N/A')`  
------------latlong(WGS84)  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Takes the coordinates from the internal GPS.  
------------------function ->  
------------------CASE  
                     WHEN @gnss_coordinate IS NULL THEN NULL  
                     ELSE concat(replace(format_number(y(@gnss_coordinate), 6), ',', '.'),', ',replace(format_number(x(@gnss_coordinate), 6), ',', '.'))  
                  END  
------------altitude  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// Takes the altitude from the internal GPS  
------------------function ->  
------------------CASE  
                     WHEN @gnss_coordinate IS NULL THEN NULL  
                     ELSE replace(format_number(z(@gnss_coordinate), 3),',','.')  
                  END  
------------filename  
---------------Editable: yes  
---------------Widget: Text Edit  
---------------Default value:  
------------------// The filename of the photo  
------------------function -> `file_name("photo")`  
---------------Apply default value on update  
------------------Yes  
------------Scope  
---------------Editable: no  
---------------Widget: Text Edit  
---------------Default value:  
------------------// The creator of this guide  
------------------function -> `'© Utrecht University'`  
---------------Apply default value on update  
------------------Yes  
------Qfield  
---------Attachment settings  
------------photo  
---------------// Saves the photo files in the correct sub-folder with timestamp as name  
---------------function -> `'photos/photos_' || format_date(now(),'yyyyMMddhhmmsszzz') || '.{extension}'`  
---lines  
------Symbiology  
---------Categorized, Value: Type, add by pressing the plus symbol  
------------// Make your own categories with symbols based on the `lines.csv` in the suplementary files  
------------// ---Start with the example below (copy-aste further amd modify) and make sure that for   
------------// ---all categories the color is taken from the color field. Make sure that all types are defined  
------------Line  
---------------Simple line  
------------Stroke width  
---------------1  
------------Color  
---------------// Take the color from the color field  
---------------function -> `if("color" is not null and "color" != '', "color", @value)`  
------Attribute form, Type Drag and Drop Designer  
---------Remove fid from Drag and Drop Designer  
---------Add new tab called 'Data'. Inside put the following fields:  
------------type  
---------------Editable: yes  
---------------Widget: Value Map  
------------------Load Data from CSV file -> supplementary file file `lines.csv`  
---------------Constraints  
------------------Not null  
------------------Enforce not null constraint  
------------description  
---------------Editable: yes  
---------------Widget: Text Edit  
------------color  
---------------Editable: yes  
---------------Widget: Color  
---------------// Uses the system colors, in Qfield you get 5 colors  
---------------Default value:   
------------------function -> `'#e41a1c'`  
---polygons  
------Symbiology  
---------Single Symbol  
------------Fill  
---------------Simple Fill  
------------Stroke width  
---------------1  
------------// Stroke and (semitransparent) fill color are the same, defined by the field color  
------------Fill Color  
---------------function -> `set_color_part(if("color line" is not null and "color line" != '', "color line", @value),'alpha',77)`  
------------Stroke Color  
---------------function -> `if("color line" is not null and "color line" != '', "color line", @value)`  
------Attribute form, Type Drag and Drop Designer  
---------Remove fid from Drag and Drop Designer  
---------Add new tab called 'Data'. Inside put the following fields:  
------------name  
---------------Editable: yes  
---------------Widget: Text Edit  
------------description  
---------------Editable: yes  
---------------Widget: Text Edit  
------------color   
---------------Editable: yes  
---------------Widget: Color  
---------------// Uses the system colors, in Qfield you get 5 colors  
---------------Default value:   
------------------function -> `'#e41a1c'`  
  
## Step 6: Finalizing the project  
// Hide photos and lithology_value_relation  
Project -> Properties -> Data sources  
---make photos and lithology_value_relation Private -> OK  
Final Save your project, exit QGIS  
  
# Workflow in Qfield  
// Copy the folder `UU_GEO_fieldwork` in the `Imported datasets` folder of the Qfield app on your mobile device.   
// ---On IOS devices use the Files app to move the folder in the `Imported datasets` folder of the Qfield app.   
// ---On Android devices Qfield has a system of copying/moving the folder.  
// In Qfield settings make sure that Use native camera is enabled.   
Qfield -> Settings -> Use native camera  
---On  
  
## Start using and testing the correctness of your setup by inputting data.  
// Compare to the solution of your assignment available in the folder `UU_GEO_fieldwork`  
// ---Unzip file first and copy to your mobile device as above  
  
// At the end test and debug the project carefully!  
  
# Additional Plugins for QField and settings  
// You can modify the plugins below by downloading on your computer and modyfying their QML JAVA programming  
// Instalation on your mobile device in Qfield by copy-paste the links below in   
Qfield -> Settings -> Manage plugins -> Install plugin from URL  

## STRUCTURAL GEOLOGY CALCULATOR  
// Contains:  
// ---Tab S - Stereonet plotting of all types of structural  
//------Tabs C1-7: Rapid structural geology calculations needed in the field  
//------Operations with planes (e.g., fault and Riedel), planes and lines (e.g., fault and drag-fold),  
//------backtilting, apparent dips in cross-sections, Pi-diagram  
//------density plots of lineations and poles, paleostress axes  
// Install via the Plugin Manager of QField:  
https://github.com/liviumatenco/Qfield-Structural-Geology-Calculator/archive/refs/heads/main.zip  
  
## Automatic compass measurements with your mobile device   
//    modified from https://www.opengis.ch  
// ---Experience demonstrates that the compass of some mobile devices is NOT reliable   
// ---and/or students make many mistakes by using it.  
// ---In the field use a real compass for measurements.   
https://drive.google.com/uc?export=download&id=1Z8SIy5eYaq-TjxadDm1WlnU6CQiVg2DT  
  
## Changelog & Updates
### Version 2.0:
* **Dynamic Categories:** Replaced static characterisation of `Types` to a dynamic one
* **Generations of structures:** Assignment of levels of the same type (i.e . generations) in a `Generation` field.
* **Replaced external png symbols:** Internal QGIS/Qfield Simple symbols are now used.
* **Support for the Qfield Structural Geology Calculator, v 2.0:** Dynamic assignment of the `Types` field. See https://github.com/liviumatenco/Qfield-Structural-Geology-Calculator.
### Version 1.0: 
Initial release.
