Erika Ito
Lab Exercise 11
Problem Set 1

> 20/20

1) You first mission is to download a list of all Triassic units in the Macrostrat Database using the /units route into R. Name this object TriassicUnits. As always, show your code.
Hints:
* You will want to use the read.csv( ) function, similarly to how we have loaded in Paleobiology Database data. Review previous labs if you do not remember how to use this function.
* You will want to make sure that you set the output format to csv.
* You should use the interval name Triassic rather than the numerical ages to define your search.

`> TriassicUnits<-read.csv("http://macrostrat.org/api/units?interval_name=Triassic&format=csv")`

2) How many Triassic units did you download? What code did you use to find out?

838

````R
> dim(TriassicUnits)
[1] 838  17
````
3) Look at the first 10 units that you downloaded. Are they mostly Igneous, Metamorphic, or Sedimentary rocks? Do you believe that these units are relevant for an investigation of fossil preservation? Show your code and explain your reasoning.

`>TriassicUnits[1:10,]`

They are mostly igneous rocks This is relevant for fossil preservation because fossils are much more likely to be preserved in sedimentary or metamorphic rocks compared to igneous rocks. Igneous rocks are crystalline that form from the cooling of magma. It is unlikely that a fossil would be preserved in the very high temperatures required for an igneous rock to form. Metamorphic and sedimentary rocks to not involve any melting processes in their formation, and therefore are more likely to have fossils preserved within them. 

4) Which columns of your TriassicUnits data.frame denote the starting age and ending age of each unit, respectively?
````R
> names(TriassicUnits)
Column 12  (“t_age”) denotes the ending age
Column 12 (“b_age”) denotes the starting age
````

5) Considering the bottom and top ages of the first 10 units, are these units constrained to only the Triassic or do some range through the Triassic?
````R
> TriassicUnits[1:10,"t_age"]
 [1]  67.037  74.975  92.875  92.875  92.875
 [6]  92.875  93.900  95.550 100.500 103.625
> TriassicUnits[1:10,"b_age"]
 [1] 259.9000 396.8750 203.1000 203.1000
 [5] 203.1000 203.1000 261.9667 203.1000
 [9] 396.8750 249.1750
````

The Triassic started 252 Ma and ended 199 Ma. Based on the ages above, some of the units range through the Triassic. 

## Problem Set 2
1) Re-download TriassicUnits, however, this time restrict the data to only sedimentary rocks. Show your code.
Hints:
* /units documentation may provide some hepful information.
* /defs/lithologies may provide some helpful information.
* As before, use the read.csv( ) function and set the output format to csv.
`> TriassicUnits<-read.csv("http://macrostrat.org/api/units?interval_name=Triassic&lith_class=sedimentary&format=csv")`

2) Restrict TriassicUnits to only units that with starting ages <= start of the Triassic and ending ages >= to the end of the Triassic. As always, show your code.
Hints:
* You will want to use either the which( ) or subset( ) functions.
* You will have to make use of logical operators >=, <=, and &. For a review of these, see the logical operators andsubsetting with logical operators sections of the R Tutorial.

`> SubsetTri<-subset(TriassicUnits,TriassicUnits[,"b_age"]<=251&TriassicUnits[,"t_age"]>=199)`

3) Repeat the above processes (download and subset) for the following geologic periods. The Cretaceous, Jurassic, Triassic (you don't have to download it twice), Permian, Carboniferous, Devonian, Silurian, and Ordovician. Show your code, but do not show me the downloaded data.

````R
> CretaceousUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Cretaceous&lith_class=sedimentary&format=csv")
> SubsetCret<-subset(CretaceousUnits,CretaceousUnits[,"b_age"]<=145.5&CretaceousUnits[,"t_age"]>=65.5)
> JurassicUnits<-read.csv("http://macrostrat.org/api/units?interval_name=Jurassic&lith_class=sedimentary&format=csv")
> SubsetJur<-subset(JurassicUnits,JurassicUnits[,"b_age"]<199.6&JurassicUnits[,"t_age"]>=145.5)
> PermianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Permian&lith_class=sedimentary&format=csv")
> SubsetPer<-subset(PermianUnits,PermianUnits[,"b_age"]<=298.9&PermianUnits[,"t_age"]>=252.17)
> CarboniferousUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Carboniferous&lith_class=sedimentary&format=csv")
> SubsetCarb<-subset(CarboniferousUnits,CarboniferousUnits[,"b_age"]<=359.2&CarboniferousUnits[,"t_age"]>=299)
> DevonianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Devonian&lith_class=sedimentary&format=csv")
> SubsetDev<-subset(DevonianUnits,DevonianUnits[,"b_age"]<=419.2&DevonianUnits[,"t_age"]>=358.9)
> SilurianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Silurian&lith_class=sedimentary&format=csv")
> SubsetSil<-subset(SilurianUnits,SilurianUnits[,"b_age"]<=443.8&SilurianUnits[,"t_age"]>=419.2)
> OrdovicianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Ordovician&lith_class=sedimentary&format=csv")
> SubsetOrdo<-subset(OrdovicianUnits,OrdovicianUnits[,"b_age"]<=485.4&OrdovicianUnits[,"t_age"]>=443.8)
````

4) Make a vector named UnitFreqs that records the number of units present in each period. Show your code.

````R
> Vector<-c(nrow(SubsetCret),nrow(SubsetJur), nrow(SubsetTri),nrow(SubsetPer),nrow(SubsetCarb),nrow(SubsetDev),nrow(SubsetSil),nrow(SubsetOrdo))
````

5) Find the mean and standard deviation of UnitFreqs not counting the Triassic. How many standard deviations above or below the mean is the number of Triassic Units? Show your code.

The number of Triassic units is 1.19 standard deviations below the mean. 

````R
> AnotherVector<-Vector[-3]
> sd(AnotherVector)
[1] 1339.846
> mean(AnotherVector)
[1] 2105.714
> nrow(SubsetTri)
[1] 517
> 2105.714-517
[1] 1588.714
> 1588.714/1339.846
[1] 1.185744
````

6) Given your answer to the above, do you believe that the Triassic has a statistically lower number of units than the other periods? Why?

No, it is not statistically lower than the other periods. Using the 2 sigma rule (two standard deviations or ~95% confidence interval), the Triassic does not have a statistically lower number of units than other periods because it is within two standard deviations from the mean number of units of the other periods. 

7) What if you consider both the Triassic and Jurassic as potential outliers? Explain (show your code) how you arrived at your answer.

````R
> yetanothervector<-AnotherVector[-2]
> sd(yetanothervector)
[1] 1337.401
> mean(yetanothervector)
[1] 2314.333
> nrow(SubsetTri)
[1] 517
> nrow(SubsetJur)
[1] 854
> (2314.333-517)/1337.401
[1] 1.3439
> (2314.333-854)/1337.401
[1] 1.091919
````

Neither the Jurassic nor the Triassic have a statistically lower number of units than the other periods. As shown in the code above they are both within 2 standard deviations from the mean number of units for the other periods. 


## Problem Set 3
1) Download and plot a map of all North American geologic columns (no color). Show your code.

````R
> URL<-"https://macrostrat.org/api/columns?format=geojson_bare&project_id=1"
> GotURL<-getURL(URL)
> ModernMap<-readOGR(GotURL,"OGRGeoJSON",verbose=FALSE)
> plot(ModernMap,col="white")
````

2) On top of your map from Question 1, plot a map of all North American columns with Induan-Anisian sedimentary units. Use the Olenekian hexcode color for this map. You can look up the Olenekian hexcode through the /defs/intervals route.

````R
> URL2<-"https://macrostrat.org/api/columns?age_top=242&age_bottom=252.2&lith_class=sedimentary&format=geojson_bare&project_id=1"
> GotURL2<-getURL(URL2)
> AnotherMap<-readOGR(GotURL2,"OGRGeoJSON",verbose=FALSE)
> plot(ModernMap,col="white")
> plot(AnotherMap,col="#B051A5",add=TRUE)
````

3) Using the downloadPBDB( ) function from previous labs. Download all occurrences of animal fossils in the Paleobiology Database of Induan-Anisian age. Using the points( ) function, plot all of these occurrences on the map you made in question 2 as solid circles. If you do not remember how to use points, you can consult previous labs or use help(points). Show your code. Save this map for later questions.

````R
> DataPBDB<-downloadPBDB(Taxa=c("Animalia"),StartInterval="Induan",StopInterval="Anisian")
> points(x=DataPBDB[,"lng"],y=DataPBDB[,"lat"],pch = 19)
````

4) You can open a new plot window using quartz( ) if you are on a mac or `windows( ) if you are on a windows machine. Download and plot a map of all North American geologic columns (no color) - i.e., repeat Question 1 in this new plot window. On top of this map, plot a map of all North American columns with Lopingian aged sedimentary units. Use the appropriate Lopingian hexcode color for this new map. Show your code.

````R
> URL3<-"https://macrostrat.org/api/columns?interval_name=Lopingian&lith_class=sedimentary&format=geojson_bare&project_id=1"
> GotURL3<-getURL(URL3)
> Map3<-readOGR(GotURL3,"OGRGeoJSON",verbose=FALSE)
> windows(plot(ModernMap,col="white"))
> plot(Map3,col="#FBA794",add=TRUE)
````

5) Download all occurrences of animal fossils in the Paleobiology Database of Lopingian age. Plot all of these occurrences on the map you made in question 2 as solid circles. Show your code.

````R
> DataPBDB2<-downloadPBDB(Taxa=c("Animalia"),StartInterval="Lopingian",StopInterval="Lopingian")
> points(x=DataPBDB2[,"lng"],y=DataPBDB2[,"lat"],pch = 19)
````

6) Compare and contrast your Induan-Anisian map versus your Lopingian map. Does it seem based on these maps that...
* There was a substantial drop in the areal extent of North American sedimentary units across the P/T boundary.

No. Based on the maps, the areal extent of sedimentary units in North America appears to be about the same between the Late Permian and the Lower Triassic. In fact, there appears to be a slightly greater areal extent of sedimentary units in the Lower Triassic map, but this may be due to the fact that the time window captured in that map is about 2 million years longer than the Late Permian map.

+ There was a substantial drop in the percentage of sedimentary units with reported fossils in them aross the P/T boundary?
There actually appears to be an increase in percentage of sedimentary units with reported fossils in the Lower Triassic compared to the Late Permian. 

+ Overall, do you think there is sufficient evidence from these maps to reject or accept the hypothesis that lower diversity in the Early Triassic is an artefact of either poor fossil sampling of the available sedimentary rock or a low availability of sedimentary rock.

No. The map created for the Early Triassic spans from the Induan to the Anisian, a period of about ten million years. The Lopingian Map also captures a time period of about ten million years. Based on these maps alone we cannot tell whether or not poor sampling or low sedimentary rock availability might have created the illusion of lower diversity in the Early Triassic. There could have been, for example, a period of reduced sedimentation in the Early Triassic, that was not captured in the map due to the large time window for the Triassic map. There similarly could have been a period of increased sedimentation at the end of the Triassic, that was not captured for the same reason. It would be helpful to look at more maps between these intervals, that capture shorter windows of time. This may give a more detailed idea and revealing idea of how sedimentary processes were fluctuating across the Permo-Triassic boundary. Based on the maps we created alone, there is not enough evidence to reject the hypothesis of lower diversity in the Early Triassic being an artifact of undersampling or low availability of sedimentary units. 
