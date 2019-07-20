---?image=assets/img/grass_template.png&position=bottom&size=100% 30%
@title[Front page]

@snap[north span-100]
<br>
<h2>Spatio-temporal data processing & visualization in @color[green](GRASS GIS)</h2>
@snapend

@snap[south message-box-white]
<br>Dra. Verónica Andreo<br>CONICET - INMeT<br><br>Instituto Gulich. Córdoba, 2019<br>
@snapend

---

### Who are we?


put pics and affil

---

@color[#8EA33B](GRASS GIS) is **the first Open Source GIS** that incorporated
capabilities to **manage, analyze, process and visualize spatio-temporal
data**, as well as the temporal relationships among time series.

@fa[layer-group fa-3x text-green]

+++

## The TGRASS framework

@ul
- TGRASS is the temporal enabled GRASS GIS designed to easily handle time series data
- TGRASS is fully @color[#8EA33B](based on metadata) and does not duplicate any dataset
- @color[#8EA33B](Snapshot) approach, i.e., adds time stamps to maps
- A collection of time stamped maps (snapshots) of the same variable are called @color[#8EA33B](space-time datasets or STDS)
- Maps in a STDS can have different spatial and temporal extents
@ulend

<!---
TGRASS uses an SQL database to store the temporal and spatial extension
of STDS, as well as the topological relationships among maps and among
STDS in each mapset.
--->

+++

## Space-time datasets

- Space time raster datasets (**ST@color[#8EA33B](R)DS**)
- Space time 3D raster datasets (**ST@color[#8EA33B](R3)DS**)
- Space time vector datasets (**ST@color[#8EA33B](V)DS**)

@fa[layer-group fa-3x text-green]

+++

## Other TGRASS notions

@ul
- Time can be defined as @color[#8EA33B](intervals) (start and end time) or @color[#8EA33B](instances) (only start time)
- Time can be @color[#8EA33B](absolute) (e.g., 2017-04-06 22:39:49) or @color[#8EA33B](relative) (e.g., 4 years, 90 days)
- @color[#8EA33B](Granularity) is the greatest common divisor of the temporal extents (and possible gaps) of all maps in the space-time cube
@ulend

+++

### Other TGRASS notions

- @color[#8EA33B](Topology) refers to temporal relations between time intervals in a STDS.

<img src="assets/img/temp_relation.png">

+++

### Other TGRASS notions

- @color[#8EA33B](Temporal sampling) is used to determine the state of one process during a second process.

<img src="assets/img/temp_samplings.png" width="55%">

+++

## @fa[tools text-green] Spatio-temporal modules

- @color[#8EA33B](**t.\***): General modules to handle STDS of all types
- @color[#8EA33B](**t.rast.\***): Modules that deal with STRDS
- @color[#8EA33B](**t.rast3d.\***): Modules that deal with STR3DS
- @color[#8EA33B](**t.vect.\***): Modules that deal with STVDS

---?image=assets/img/grass_template.png&position=bottom&size=100% 30%

## TGRASS framework and workflow

+++?image=assets/img/tgrass_flowchart.png&position=center&size=auto 93%

---?image=assets/img/grass_template.png&position=bottom&size=100% 30%

## Hands-on to raster time series in GRASS GIS

---

### @fa[download] get the data and the code @fa[download]
<br>
- [modis_lst mapset (2Mb)](https://gitlab.com/veroandreo/grass-gis-geostat-2018/blob/master/data/modis_lst.zip): download and unzip within `$HOME/grassdata/nc_spm_08_grass7`
- [GRASS code](https://gitlab.com/veroandreo/grass-gis-conae/raw/master/code/05_temporal_code.sh?inline=false)
- [R code](https://gitlab.com/veroandreo/grass-gis-conae/raw/master/code/05_temporal_code.r?inline=false)

... and start GRASS GIS

```bash
grass76 $HOME/grassdata/nc_spm_08_grass7/modis_lst --gui
```

---?code=code/05_temporal_code.sh&lang=bash&title=Set computational region and apply MASK

@[32-40](List raster maps and get info)
@[43-61](Set computational region)
@[63-67](Set a MASK)

---

### Create a temporal dataset (STDS)

**[t.create](https://grass.osgeo.org/grass76/manuals/t.create.html)**
<br>
- Creates an SQLite container table in the temporal database 
- Handles huge amounts of maps by using the STDS as input 
- We need to specify:
  - *type of maps* (raster, raster3d or vector)
  - *type of time* (absolute or relative)

+++?code=code/05_temporal_code.sh&lang=bash&title=Create a raster time series (STRDS)

@[70-75](Create the STRDS)
@[77-78](Check if the STRDS is created)
@[80-81](Get info about the STRDS)

---  

### Register maps into the STRDS

**[t.register](https://grass.osgeo.org/grass76/manuals/t.register.html)**
<br>
- Assigns time stamps to maps
- We need: 
  - the *empty STDS* as input, i.e., the container table, 
  - the *list of maps* to be registered, 
  - the *start date*,
  - *increment* option along with the *-i* flag for interval creation 

+++?code=code/05_temporal_code.sh&lang=bash&title=Register maps in STRDS (assign time stamps)

@[84-89](Add time stamps to maps, i.e., register maps - *nix)
@[91-94](Add time stamps to maps, i.e., register maps - windows)
@[96-97](Check info again)
@[99-100](Check the list of maps in the STRDS)
@[102-103](Check min and max per map)

@size[20px](For more options, check the <a href="https://grass.osgeo.org/grass76/manuals/t.register.html">t.register</a> manual and related <a href="https://grasswiki.osgeo.org/wiki/Temporal_data_processing/maps_registration">map registration wiki</a> page.)

+++?code=code/05_temporal_code.sh&lang=bash&title=Graphical Representation of the STRDS

@[106-107](Graphical representation of the time series)

+++

<img src="assets/img/g_gui_timeline_monthly.png" width="70%">

@size[24px](See <a href="https://grass.osgeo.org/grass76/manuals/g.gui.timeline.html">g.gui.timeline</a> manual page)

---

@snap[north span-100]
<h3>Operations with temporal algebra</h3>
@snapend

@snap[south list-content-verbose span-100]
**[t.rast.algebra](https://grass.osgeo.org/grass76/manuals/t.rast.algebra.html)**
<br><br>
@ul[](false)
- Performs a wide range of temporal and spatial map algebra operations based on map's temporal topology 
@ul[](false)
  - Temporal operators: union, intersection, etc.
  - Temporal functions: *start_time()*, *start_doy()*, etc.
  - Spatial operators (subset of [r.mapcalc](https://grass.osgeo.org/grass76/manuals/r.mapcalc.html))
  - Temporal neighbourhood modifier: *[x,y,t]*
  - Other temporal functions like *tsnap()*, *buff_t()* or *tshift()*
@ulend
@ulend
**@size[30px](they can be combined in complex expressions!!)** @fa[bomb] 
@snapend

+++?code=code/05_temporal_code.sh&lang=bash&title=From K*50 to Celsius using the temporal calculator

@[110-114](Re-scale data to degrees Celsius)
@[116-117](Check info)

+++?code=code/05_temporal_code.sh&lang=bash&title=Time series plot

@[120-127](LST time series plot for the city center of Raleigh)

@size[20px](For a single point, see <a href="https://grass.osgeo.org/grass76/manuals/g.gui.tplot.html">g.gui.tplot</a>. For a vector of points, see <a href="https://grass.osgeo.org/grass76/manuals/t.rast.what.html">t.rast.what</a>.)

+++

<img src="assets/img/g_gui_tplot_final.png" width="80%">

@size[24px](Point coordinates can be typed directly, copied from the map display and pasted or directly chosen from the main map display.)

---

#### Lists and selections

- **[t.list](https://grass.osgeo.org/grass76/manuals/t.list.html)** for listing STDS and maps registered in the temporal database,
- **[t.rast.list](https://grass.osgeo.org/grass76/manuals/t.rast.list.html)** for maps in raster time series, and
- **[t.vect.list](https://grass.osgeo.org/grass76/manuals/t.vect.list.html)** for maps in vector time series.

<!--- list of variables to use for query 
id, name, creator, mapset, temporal_type, creation_time, start_time, end_time, north, south, west, east, nsres, ewres, cols, rows, number_of_cells, min, max
id, name, layer, creator, mapset, temporal_type, creation_time, start_time, end_time, north, south, west, east, points, lines, boundaries, centroids, faces, kernels, primitives, nodes, areas, islands, holes, volumes
--->

+++?code=code/05_temporal_code.sh&lang=bash&title=Listing examples

@[132-143](Maps with minimum value lower than or equal to 5)
@[145-158](Maps with maximum value higher than 30)
@[160-168](Maps between two given dates)
@[170-177](Maps from January)

---?code=code/05_temporal_code.sh&lang=bash&title=Descriptive statistics of LST time series

@[182-190](Print univariate stats for maps within STRDS)
@[192-193](Get extended statistics)
@[195-197](Write the univariate stats output to a csv file)

---

### Temporal aggregation 1: Using the full time series

**[t.rast.series](https://grass.osgeo.org/grass76/manuals/t.rast.series.html)**
<br>
- Aggregates full STRDS or parts of it using the *where* option
- Different methods available: average, minimum, maximum, median, mode, etc.

+++?code=code/05_temporal_code.sh&lang=bash&title=Maximum and minimum LST in the past 3 years

@[202-204](Get maximum LST in the STRDS)
@[206-208](Get minimum LST in the STRDS)
@[210-211](Change color pallete to celsius)

+++?code=code/05_temporal_code.sh&lang=bash&title=Compare maps with the Mapswipe tool

@[214-220](Display the new maps with mapswipe and compare them to elevation)

+++

![mapswipe and lst max](assets/img/g_gui_mapswipe_lstmax.png)

+++

![mapswipe and lst min](assets/img/g_gui_mapswipe_lstmin.png)

---

### Temporal operations using time variables

**[t.rast.mapcalc](https://grass.osgeo.org/grass76/manuals/t.rast.mapcalc.html)**
<br>
- Performs spatio-temporal mapcalc expressions
- It allows for *spatial and temporal operators*, as well as *internal variables* in the expression string
- The temporal variables include: *start_time(), end_time(), start_month(), start_doy()*, etc. 

+++?code=code/05_temporal_code.sh&lang=bash&title=Which is the month of the maximum LST?

@[225-228](Get month of maximum LST)
@[230-231](Get basic info)
@[233-234](Get the earliest month in which the maximum appeared)
@[236-241](Remove month_max_lst strds)

@size[24px](We could do this year-wise to know when the annual max LST occurs and assess trends)

+++?code=code/05_temporal_code.sh&lang=bash&title=Display the resulting map from the CLI

@[244-247](Open a monitor)
@[249-250](Display raster map)
@[252-253](Display only boundary of vector map)
@[255-257](Add raster legend)
@[259-260](Add scale bar)
@[262-263](Add North arrow)
@[265-267](Add title text)

+++

![Month of maximum LST](assets/img/month_max_lst.png)

---

### Temporal aggregation 2: using granularity

**[t.rast.aggregate](https://grass.osgeo.org/grass76/manuals/t.rast.aggregate.html)**
<br>
- Aggregates raster maps in STRDS with different **granularities** 
- *where* option allows to set specific dates for the aggregation
- Different methods available: average, minimum, maximum, median, mode, etc.

+++?code=code/05_temporal_code.sh&lang=bash&title=From monthly to seasonal LST

@[270-276](3-month mean LST)
@[278-279](Check info)
@[281-296](Check map list)

+++

> @fa[tasks] **Task**: Compare the monthly and seasonal timelines with [g.gui.timeline](https://grass.osgeo.org/grass76/manuals/g.gui.timeline.html)

<!---
```bash
g.gui.timeline inputs=LST_Day_monthly_celsius,LST_Day_mean_3month
```
--->

+++?code=code/05_temporal_code.sh&lang=bash&title=Display seasonal LST using frames in wx monitor

@[299-302](Set STRDS color table to celsius degrees)
@[304-306](Start a new cairo monitor)
@[308-312](Create first frame)
@[314-318](Create second frame)
@[320-324](Create third frame)
@[326-330](Create fourth frame)
@[332-333](Release monitor)

+++

![Sesonal LST by frames](assets/img/frames.png)

@size[24px](3-month average LST in 2015)

---

> @fa[tasks] **Task**: Now that you know [t.rast.aggregate](https://grass.osgeo.org/grass76/manuals/t.rast.aggregate.html), 
> extract the month of maximum LST per year and then test if there's any positive or 
> negative trend, i.e., if maximum LST values are observed later or earlier with time (years)

+++

One solution could be...
<br>
```bash
t.rast.aggregate input=LST_Day_monthly_celsius output=month_max_LST_per_year \
  basename=month_max_LST suffix=gran \
  method=max_raster granularity="1 year" 

t.rast.series input=month_max_LST_per_year output=slope_month_max_LST \
  method=slope
```

---

### Animations

![Animation 3month LST](assets/img/3month_lst_anim_small.gif)

+++?code=code/05_temporal_code.sh&lang=bash&title=Animation of seasonal LST time series

@[336-339](Animation of seasonal LST)

@size[20px](See <a href="https://grass.osgeo.org/grass76/manuals/g.gui.animation.html">g.gui.animation</a> manual for further options and tweaks)

---

@snap[north span-100]
<h3>Aggregation vs Climatology</h3>
@snapend

@snap[west span-45]
<img src="assets/img/aggregation.png">
<br>
Granularity aggregation
@snapend

@snap[east span-50]
<img src="assets/img/climatology.png">
<br>
Climatology-type aggregation
@snapend

+++?code=code/05_temporal_code.sh&lang=bash&title=Monthly climatologies

@[344-347](January average LST)
@[349-354](Climatology for all months - *nix)
@[356-361](Climatology for all months - windows)

+++

> @fa[tasks] **Task**: Compare monthly means with "climatological" means

---

### Annual anomalies
<br>

`\[
StdAnomaly_i = \frac{Average_i - Average}{SD}
\]`

<br>
We need:

- overall average and standard deviation
- yearly averages

+++?code=code/05_temporal_code.sh&lang=bash&title=Annual anomalies

@[366-368](Get general average)
@[370-372](Get general SD)
@[374-377](Get annual averages)
@[379-381](Estimate annual anomalies)
@[383-384](Set color table)
@[386-387](Animation)

+++

![Anomalies animation](assets/img/LST_anomalies.gif)

---

### Zonal statistics in raster time series

**[v.strds.stats](https://grass.osgeo.org/grass7/manuals/addons/v.strds.stats.html)**
<br>
- Allows to obtain spatially aggregated time series data for polygons in a vector map

+++?code=code/05_temporal_code.sh&lang=bash&title=Extract mean LST for Raleigh (NC) urban area

@[392-393](Install v.strds.stats add-on)
@[395-398](Extract seasonal average LST for Raleigh urban area)
@[400-403](Save the attribute table of the new vector into a csv file)

---

## QUESTIONS?

<img src="assets/img/gummy-question.png" width="45%">

---

## Other (very) useful resources

- [Temporal data processing wiki](https://grasswiki.osgeo.org/wiki/Temporal_data_processing)
- [GRASS GIS and R for time series processing wiki](https://grasswiki.osgeo.org/wiki/Temporal_data_processing/GRASS_R_raster_time_series_processing)
- [GRASS GIS temporal workshop at NCSU](http://ncsu-geoforall-lab.github.io/grass-temporal-workshop/)
- [GRASS GIS workshop held in Jena 2018](http://training.gismentors.eu/grass-gis-workshop-jena-2018/index.html)
- [GRASS GIS course IRSAE 2018](http://training.gismentors.eu/grass-gis-irsae-winter-course-2018/index.html)
- [GRASS GIS course in Argentina 2018](https://gitlab.com/veroandreo/curso-grass-gis-rioiv)

---

## References

- Gebbert, S., Pebesma, E. (2014). *A temporal GIS for field based environmental modeling*. Environmental Modelling & Software, 53, 1-12. [DOI](https://doi.org/10.1016/j.envsoft.2013.11.001)
- Gebbert, S., Pebesma, E. (2017). *The GRASS GIS temporal framework*. International Journal of Geographical Information Science 31, 1273-1292. [DOI](http://dx.doi.org/10.1080/13658816.2017.1306862)
- Gebbert, S., Leppelt, T. and Pebesma, E. (2019). *A Topology Based Spatio-Temporal Map Algebra for Big Data Analysis*. Data, 4, 86. [DOI](https://doi.org/10.3390/data4020086)

---?image=assets/img/grass_sprint2018_bonn_fotowall_medium.jpg&size=cover

@transition[zoom]

<p style="color:white">Join and enjoy GRASS GIS!!</p>

---

**Thanks for your attention!!**

![GRASS GIS logo](assets/img/grass_logo_alphab.png)

---

@snap[south span-50]
@size[18px](Presentation powered by)
<br>
<a href="https://gitpitch.com/">
<img src="assets/img/gitpitch_logo.png" width="20%"></a>
@snapend