---
title: Querying OSM data in QGis and Why it Hangs Forever
description: QGis natively supports connecting to a PostGIS database, but if you do not know the correct boxes to check, it will hang.
tags: qgis osm map
---

[OpenStreetMap](https://openstreetmap.org) is the source for free and open-source maps and geospatial information.
It contains a true wealth of knowledge on the world, from roads to businesses, opening times, contact information and so much more.
One can relatively easily import this data into a [PostGIS](https://postgis.net) database and build their own applications using this data.
When developing queries it can however be very useful to not just see a table of results, but display these on a map.
That is what I set out to achieve using [QGis](https://qgis.org).

## QGis

QGis is an open source GIS tool that supports many different data sources and a plethora of tools to view and edit geospatial information.
Most importantly for us, it natively supports connecting to a PostGIS database, executing queries on it and displaying these on a basemap of choice.
Setting up such a basemap is very easy. I used the official pre-rendered OpenStreetMap tiles, so I just expanded the _XYZ Tiles_ tree in the browser and selected _OpenStreetMap_.
Now we are seeing something.

## Connecting PostGIS

Connecting to PostGIS is also, seemingly, very easy. In the same browser, we right click on _PostgreSQL_ and choose _New connection_.
We enter a name, port, database and store our login in the password-protected configuration. Test the connection, all looks good.
Click OK and... QGis hangs. 

As it turns out, by default when connecting to PostgreSQL / PostGIS, QGis will attempt to load a whole lot of metadata.
Even for a smallish database like mine -- only containing Europe --, this takes longer than anyone has patience for.
Instead, we can tick the _Use estimated table metadata_ to speed this up dramatically.
We now have a list of the schemas and can explore tables. Great success. Or is it?

## Adding a query layer

Showing the results of a SQL query on the map is also easily enough done.
Right click the database connection, select _Execute SQL_, enter a query and press _Execute_ to test it.
What's that, it hangs already?

If QGis didn't hang just now, that means you most likely did not put a semicolon at then end of your query.
Despite what you might have learnt about SQL requiring semicolons, you should not put one here.
It turns out QGis essentially copy-pastes your query text into a subquery.
Having a semicolon there results in an invalid SQL syntax, and the subsequent error is not properly handled.
If you directly clicked "Add layer", odds are it would have just generated a broken layer instead of getting stuck.
There is already a [GitHub issue](https://github.com/qgis/QGIS/issues/56993) to track this.

Okay, so no semicolon, what else? Well if you chose the appropriate geometry and unique id columns in the _Load as new layer_ foldout and clicked _Load layer_,
you might still be stuck with a hung QGis.
Why this time?
The obvious case is that your query returned too much data, but you could probably figure that out on your own.
What is less obvious, is that `SELECT *` will destroy you.
Why this is, is between QGis and God I'm afraid.
Instead, specify all the columns you want yourselves, and specify as few as possible to speed up the situation.

## Phew

We're done here, finally. Did you have a good time?
