# Drone-Flight-Mapping

## Introduction
This is a tutorial for creating offline maps specifically related to drone flight planning. A few points:
* This assumes Windows 10 and all instructions are based on this, other platforms may act slightly differently.
* I am a Mavic Air 2 pilot in the UK and distances etc are based on my restrictions for example use only and should be adjusted according to your specific local regulations.
* **Use these instructions at your own risk** - this is for informational purposes only and nothing in this implementation, nor its output, should be taken as authorization to fly in any specific area. This is simply a tool to *assist you in the decisions you make and are responsible for*. 
* This process uses Open Source software which will need to be installed on your machine.

## Step One - install and configuring QGIS
QGIS is an open source mapping tool which will the bulk of this process will be done in. There may be some steps which offer little explanation - this is on purpose. QGIS can be a bit daunting purely due to its complexity so I've tried to keep this complexity out of this document. Where feasible I'll include links to appropriate documentation should you want to read more about certain aspects.


* Download the latest version of QGIS from the [QGIS website](https://qgis.org/en/site/forusers/download.html) (this tutorial is based off of version 3.16).
* Install the application with default settings throughout the installer.
* Once instaleld launch QGIS3.

Next we're going to add Open Street Maps to QGIS, which will provide mapping capabilities.
* On the left pane right-click XYZ Tiles and select "New Connection..."
* In the Name field type `OpenStreetMap`.
* In the URL field type `https://tile.openstreetmap.org/{z}/{x}/{y}.png`
* Click OK.

## Step Two - Getting your data
Here we will be downloading data from OpenStreetMap for us to use. We'll be downloading a lot more data than we'll use in this tutorial, however if you feel confident this means that you can later add other features to your map.

**Important note:** The larger the area you select the larger the data set will be. I would highly recommend downloading only a small area first to follow this tutorial along with. You can always revisit this step later and expand the selected area later.

* Visit [Protomaps](https://protomaps.com/extracts) and locate the area you want to download data for by zooming in on the map.
* Once you have the area you're interested in in view, click the `Draw Polygon` button. You'll now start to draw a polygon around the area you want to download data for. Left-click on the map to draw your first point, then click elsewhere to add a second point, and continue this until you've enclosed your area of interest, making sure that you left-click on the first point you made at the end to complete the polygon.
* Click Create Extract and it will begin building a file for you.
* Once your data has been generated, click the `Download .OSM.PBF` button to download the data to your machine (this may take a while).

## Step Three - Building your map
Now for the fun bit! We're going to create our base map and add the data we just downloaded to it. We're then going to narrow down the data to just show buildings in the dataset.

Our map will be based on Layers, each of which you'll see nin the lower-left corner of the QGIS window. Each of these layers can be made visible or invisible by toggling the tick mark next to that layer. The order of each layer matters as well, with layers that are higher up the list being on top of those that are lower.

* Back in QGIS, right-click the `OpenStreetMap` connection we made in `XYZ Tiles` earlier and choose `Add Layer to Projec`, after which you'll see a map of the earth on the right.
* Zoom in to the same area you downloaded the data for.
* In the `Layer` menu choose `Add Layer` followed by `Add Vector Layer...`.
* Click `...` next to `Vector Dataset(s)` and find the file you downloaded earlier, clicking OK to once its selected.
* Leave all the `Options` entries as they are and click `Add` at the bottom of the screen.
* In the dialog that appears, click the `Select All` button to ensure all the data is selected, and make sure that `Add layers to group` is ticked. Click `OK` and then click `Close` to close the `Data Source Manager` window.

In the `Layers` portion of the window (lower-left) you'll now see a whole bunch of layers that have been added to the map.

We now have our data loaded. Yay! Now we need to filter this data to something approaching usable. In this instance we'll be narrowing down the data to only show buildings, but again, feel free to come back to this step and have a play if other features are useful to you.

* At this point you may want to zoom in to your map so you can see the effect of the changes we're about to make.
* In the `Layers` pane, untick the following layers (leaving `multipolygons` ticked):
* *`lines`
* *`multilinestrings`
* *`points`
* Right-click the `multipolygons` layer and select `Filter...`.
* In the `Provider Specific Filter Expression` area at the bottom of the window, type the following:
* *`"building" != 'NULL'`
* This query is basically saying 'only include data which explicitly says this data is related to a building'.
* *On the left pane of the `Query Builder` window you'll see a lot of different types of data fields which are available in this dataset. If you want to add different data to the map you can select a `Field` from the left and click the `All` button on the right to see what kinds of data is available to you.
* Click the `OK` button to return to the main QGIS window.
* Hopefully you should now see that the data layer we've added to the map now shows only buildings.

## Step Four - Creating a permanent Buildings layer
This step is... weird. I'm not going to go in to the intricacies here as to why this is important, but its needed for the following step (Adding Boundaries). If this step isn't followed, your boundaries will probably end up in the middle of the ocean!
* Select your `multipolygons` layer so its highlighted.
* In the `Processing` menu click `Toolbox`. You'll see a `Processing Toolbox` window appear on the right of the screen.
* In the `Search` bar of the `Processing Toolbox` window, type `reproject`.
* Right click `Reproject layer` and click `Execute...`.
* Click the `Select CRS` icon to the right of the `Target CRS` dropdown.
* In the `Filter` bar type `
