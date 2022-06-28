# Spatial Data for Incident Response (Cave Rescue)

As seen at ResCon22 (British Cave Rescue Council Conference, 2022) [```ResCon22 slides```](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/raw/main/Files/ResCon22_Redacted.pdf) - my contact details are included.

If you are a search and rescue team who would like help implementing this methodology or with any time-consuming or complex geospatial data tasks, I would be happy to help - contact me directly.

Any bugs or improvements can be added to this repository as an issue, or you can contact me directly and I will do this for you to make other users aware. All major updates will be posted as issues, so check that tab for the latest updates.



### Rationale

Cave Rescue teams have a unique use case regarding spatial data, specifically the need to rapidly share and navigate to often predetermined and precise incident locations. This provides the opportunity to accurately record these locations and make them readily accessible in advance of incidents occurring, allowing incident controllers access to detailed location information ready for simple, rapid and error-free sharing with team members and other emergency services. These locations comprise of cave and mine Access Points (APs) and Rendezvous points (RVs).

The free and open source methodology (Great Britain only) below describes how to:
1. **Collate** AP and RV location data from various sources into a simple structured format
2. **Augment** these data with spatial conversions and metadata (listed below)
3. **Serve** the augmented data on a map in a way that the underlying information can be interrogated

Why a cave rescue team would want to use this methodology:
- a catalogue of APs and RVs forms the foundation for intelligent incident response planning
- viewing the data on interactive maps with satellite imagery or topographic maps allows incident controllers to view the spatial relationship between APs and RVs to identify what RVs are best positioned for the task at hand
- having multiple pre-calculated spatial conversions that describe the same location facilitates error-free information sharing
- Google Maps URLs allow automated navigation to RVs and provide an accurate assessment of ETA accounting for current road conditions, in addition to allowing incident controllers to assess RV suitability on Google Street View if available
- road access type provides an assessment of road accessibility at RVs before team members arrive, including identifying restricted access roads which may require a third party for access
- mobile phone coverage provides an assessment of available communications at RVs before team members arrive
- the data are exported in a variety of formats for use with mobile and GPS devices, online maps and GIS software
- Google cloud tools (Colaboratory, Drive and Maps) allow users to implement the methodology regardless of their technical experience or hardware and software constraints, therefore this methodology requires a free Google account



### Methodology

![Img](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/blob/main/Images/Overview.png)

*Data collation (red boxes), and serving (blue boxes) are the responsibility of a team data controller, with manual iterative maintenance steps shown (red arrows)*
  
1. **Data Collation** - Download and install [QGIS](https://qgis.org/en/site/), then download and open [```Mapping.qgz```](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/raw/main/Files/Mapping.qgz), a QGIS project containing various web map services for remote location mapping. Other data sources will be available to populate location coordinates, such as GPS measurements which capture longitude and latitude natively. Through the Plugins menu within QGIS, install the Lat Lon Tools plugin which can extract longitude, latitude coordinates (shown below).

    ![Img](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/blob/main/Images/ExtractCoordinates.png)

    *Extracting longitude, latitude coordinates for a location in QGIS using the Lat Lon Tools plugin. Ensure you have the correct settings (red) before extracting coordinates by mouse click on the map (blue)*

    Download the spreadsheet templates ([```APs_Master.ods```](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/raw/main/Files/APs_Master.ods), [```RVs_Master.ods```](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/raw/main/Files/RVs_Master.ods)) and add records to the appropriate template depending on the location type, completing the essential columns listed below (additional columns will be unaltered and will carry through).
    - ID: must be unique free text
    - Name: free text
    - LongLat: x,y format in decimal degrees
    - VerifiedDate: dd/mm/yyyy



2. **Data Augmentation** - Visit [![Augment](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/EdwardALockhart/SpatialDataIncidentResponse/blob/main/Files/Augment.ipynb) and follow the instructions to add spatial conversions and metadata to the location records which are exported in a variety of formats (.csv, .gpkg, .gpx) to "_SpatialData/Processed" on Google Drive. ```Augment.ipynb``` is executed within [Google Colaboratory](https://colab.research.google.com/), where python code can be run temporarily on a virtual machine through a web browser.

    The code uses data available only for Great Britain from the Ordnance Survey (GB postcodes and roads), therefore areas outside Great Britain are unsupported.

    - Spatial conversions (APs and RVs)
        - Longitude - decimal degrees (WGS84)
        - Latitude - decimal degrees (WGS84)
        - Easting - metres (British National Grid)
        - Northing - metres (British National Grid)
        - GridRef1m - Ordnance Survey 1m grid reference
        - What3Words - What3Words address
        - GoogleMapsURL - Google Maps URL for directions and Street View
    - Metadata (RVs only)
        - RoadAccess - nearest road access type (within 50 m) - see the [user guide](https://www.ordnancesurvey.co.uk/documents/os-open-roads-user-guide.pdf) for all value definitions under "RoadFunctionValue"
        - Postcode - nearest postcode (within 300 m)
        - MobileCoverage - minimum outdoor mobile phone coverage for all providers at buildings within a postcode - see below for definitions
        
          ![Img](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/blob/main/Images/MobileCoverage.png)



3. **Data Serving** - Google Maps can read .csv files directly from Google Drive to create a private online map where location attributes can be interrogated and searched in a web browser. Other avenues of data serving include loading .gpkg files into QGIS for viewing with other spatial data on a local PC, or loading .gpx files onto GPS devices.

    **Google Maps**

      ![Img](https://github.com/EdwardALockhart/SpatialDataIncidentResponse/blob/main/Images/NWCROmap.png)
      
      *A completed Google Map displaying North Wales APs and RVs with historical data*
      
      For map creation and other functionality instructions, visit the [Google My Maps support page](https://support.google.com/mymaps/?hl=en#topic=3024969). Any created map will exist within the Google Drive of the Google user who created it. When using the inbuilt share functionality to share the map, give viewer (not editor) permissions with any email addresses as required, they will be required to register for a free Google account with the corresponding email address to get access. Editor permissions allow users to remove and edit points within the web browser. Permissions for individual users can also be revoked or elevated within the share menu - useful for temporarily sharing a map.
