## Problem Set 1

> 19/20

There are four columns in `DataPBDB` relevant to the age of an organism: early_interval, late_interval, max_ma, and min_ma. Because we rarely have a precise date, we generally give the age of an occurrence as a range. This range can be expressed by interval names or by numbers.

1) What do the max_ma and min_ma columns of DataPBDB represent? If you do not intuitively know, you can always check the Paleobiology Database API documentation.

max_ma is the start date (in millions of years ago) and min_ma is the end date (in millions of years ago). 

2) What is oldest age of each genus? [Hint: Use the tapply( ) and max( ) functions we've used in previous labs]. Show the code you would use to find out.

````R
> tapply(DataPBDB[,"max_ma"],DataPBDB[,"genus"],max)
````

3) What is the youngest age of each genus? [Hint: Use the tapply( ) and max( ) functions we've used in previous labs]. Show the code you would use to find out.

````R
> tapply(DataPBDB[,"min_ma"],DataPBDB[,"genus"],min)
````

4) Find which genus has the most occurrences in the dataset [Hint: Use the table( ) function!]. What code did you use?
Anadara

````R
>sort(table(DataPBDB[,"genus"]))
> YAY<-sort(table(DataPBDB[,"genus"]))
> dim(YAY)
[1] 1018
> YAY[1018]
Anadara 
   1916
5) What is the stratigraphic range of this taxon (i.e., your answer to question 4). Show your code
> WOW<-subset(DataPBDB,DataPBDB[,"genus"]=="Anadara")
> max(WOW[,"max_ma"])
[1] 66
> min(WOW[,"min_ma"])
[1] 0
> 66-0
[1] 66
````

## Problem Set 2
1) Qualitatively describe what is happening in the following line of code. A good answer should identify what the different arguments are for each function, and what they are used for.

`mean(sample(Lucina[,"paleolng"],length(Lucina[,"paleolng"]),replace=TRUE))`

This code randomly resamples Lucina occurrences to find the mean paleolongitude of the resample.

+ mean() finds the mean of the inside of the parentheses (of average paleolongitude for Lucina resmple)
+ sample() takes a sample of a size specified by set.seed from the elements inside the parentheses  (inside parentheses = the paleolongitude for each Lucina occurrences)
+ (Lucina[, “Paleolng”]) calls the paleolongitude column for Lucina
+ ,length(Lucina[,”paleolng”] specifies that the number of items chosen from the paleolongitude column for Lucina should be equal to the number of elements in the entire Lucina paleolongitude column. 
+ ,replace=TRUE – specifies that the elements that were sampled can be re-sampled

2) Plot a kernel density graph of ResampledMeans. Show your code. Does the distribution look approximately Gaussian? Explain why you think it does or does not.

Yes it does look approximately Gausssian. Because it resembles a bell-curve. It appears to be symmetrical about a paleolongitude value for Lucina that is approximately the mean, median, and the mode of the distribution.  

`> plot(density(ResampledMeans))`

3) Find the mean of `ResampledMeans`, is it similar to the mean of the original data?

Yes! They are quite similar. 

````R
> mean(ResampledMeans)
[1] 24.16227
> OriginalMean
[1] 24.1997
````

4) Sort ResampledMeans from lowest to highest. [Hint: We learned how to sort a vector in Lab 6].

`> sort(ResampledMeans)`

5) Now that you have sorted ResampledMeans, what is the 2.5th percentile of ResampledMeans and what is the 97.5th percentile of Resampled means. If you do not know what a percentile is, or how to calculate it, you can use google. Show your code.

````R
> THIS<-sort(ResampledMeans)
> quantile(THIS,c(.025,.975))
    2.5%    97.5% 
21.82094 26.28630
````

6) Incidentally, these numbers (your answer to question 5) are the lower and upper confidence interval of the mean! Qualitatively explain why this is the case.

This is the confidence interval because there is a 95% chance that the true mean paleolatitude of Lucina occurrences lies within between the 2.5th percentile and 97.5th percentile values of resampled means. Essentially, we are chopping off the two end tails of the Gaussian(ish) plot coded for above 

## Problem Set 3
1) Based on the confidence intervals given above, do you think it likely or unlikely that Lucina is still alive?

Lucina is probably still alive. The earliest extinction date is zero, which means it is probably not yet extinct. The latest extinction date is calculated as -1.221208 which is 1.2 million years into the future. The extinction of Lucina has therefore probably not occurred yet. 

2) Find the extinction confidence interval for the genus Dallarca. Show your code.

````R
> Dallarca<-subset(DataPBDB,DataPBDB[,"genus"]=="Dallarca")
> estimateExtinction(Dallarca[,"min_ma"],0.95)
Earliest   Latest 
2.58800 -3.88749
````

3) A pure reading of the fossil record says that Dallarca went extinct at the end of the Pliocene Epoch. Based on its confidence interval, do you think it is possible that Dallarca is still extant (alive)?

Yes, it is possible. The earliest extinction date is at the end of the Pliocene (2.58 million years ago) according to the extinction confidence interval calculated above. However, the latest extinction date is calculated as 3.9 million years into the future. Therefore, it is possible based on the confidence interval that Dallarca is still extant.

4) In this case, should we trust the confidence interval or a pure reading of the fossil record? Explain your reasoning.
	
To speculate as to whether or not a genus is still extant, I would trust the reading from the fossil record over the confidence interval results. A 95% confidence interval means that there is a 95% chance that the true extinction date lies between the earliest and latest time periods given. <strike>This also means the likelihood that the latest extinction date given by the 95% confidence interval is the true date of extinction is not likely. In fact, using the Dallarca as an example again, there is a 74% chance based on this confidence interval calculation that the Dallarca is not extant today:</strike>

````R
> estimateExtinction(Dallarca[,"min_ma"],0.7486611668)
    Earliest       Latest 
2.588000e+00 2.634968e-10 
````

This means that it is more likely that the Dallarca is indeed extinct. So the pure reading will probably be more reliable in this case. So to speculate as to whether or not a genus is still extant I would use the pure reading. 
      
However, to pinpoint when an extinction occurred, I would use the confidence interval over the pure reading. This is because it is highly unlikely, especially for rarer genera or species, to capture the last occurrence of a taxa through sampling. Therefore, it is most likely that using the last occurrence of a taxa as the extinction date will most likely truncate, or underestimate its true stratigraphic range. Using a confidence interval, as said before gives some information about a statistically probable range in which the true extinction date falls. This seems to be a more accurate way to estimate the extinction date than to use the pure data. 

> I found it very difficult to follow your reasoning. There was a lot of double talk in this answer. -1 Points

## Problem Set 4

The problem with the `estimateExtinction( )` function is that it makes an important assumption that is unlikely to be true.

It assumes randomly distributed fossils. In other words, the fossil occurrences of taxa are randomly distributed in time, and the likelihood of preservation does not change up or down a stratigraphic section.

For this reason, paleobiologists have developed more sophisticated versions of confidence interval analysis. However, for simplicity, we will stick with the Strauss and Sadler (1987) method.

1) State one ecological reason why this assumption is unlikely to be true.
	
This is not likely to be true because there will likely be more fossil occurrences in specific time intervals due to environmental conditions during that time that favored the specific taxa. This means that there are likely intervals with many occurrences, and intervals with very few occurrences, reflecting the increased or decreased favorability of the environment for that taxa at the time. Therefore, the fossils are probably not randomly distributed in time. 

2) State one geological reason why this assumption is unlikely to be true.

This is not likely to be true because this assumption of a constant likelihood of preservation requires that sedimentation and erosional and weathering processes influence the rock record at constant rates. This assumption does not account for the gaps in the rock record and biases in fossil and rock preservation through time. 


## Problem Set 5
1) How many occurrences are in DataPBDB. How many are in ExtantData? How many occurrences were lost by limiting our anaysis to only extant bivalves?

````R
> dim(DataPBDB)
[1] 67617    26
> dim(ExtantData)
[1] 59096    26
> 67617-59096
[1] 8521
````

2) How many unique( ) genera were in DataPBDB and ExtantData, respectively. Using this information, what percentage of Cenozoic bivalves in the PBDB are still extant today.
52.3% of Cenozoic bivalves in the PBDB are still extant. 

````R
> length(unique(DataPBDB[,"genus"]))
[1] 1018
> length(unique(ExtantData[,"genus"]))
[1] 532
> 532/1018
[1] 0.5225933
````

3) Find the stratigraphic range of fossil occurrences for each genus in the ExtantData dataset. If you do not remember how to do this, revisit Problem Set 1 of this lab.

````R
>tapply(ExtantData[,"max_ma"],ExtantData[,"genus"],max)
>tapply(ExtantData[,"min_ma"],ExtantData[,"genus"],min)
`````

4) Using your answer to question 3, find which genera in ExtantData are not extant according to the PBDB - i.e., do not have a minimum min_age of zero. Show your code.

````r
> tapply(ExtantData[,"min_ma"],ExtantData[,"genus"],min)!=0
> which(THING1==TRUE)
> THING2<-which(THING1==TRUE)
>names(THING2)
````

5) Calculate the confidence interval for the extinction of the following genera (careful with your spelling!): 
Scrobicularia,Meiocardia, Dimya, Digitaria, Cuspidaria, Arctica, Aloides, Kurtiella, Gouldia, and Acrosterigma. Show your code. What percentage of these taxa have confidence intervals indicating that the taxon might still be extant?

9/10 of the taxon (90% of the taxa) have confidence intervals suggesting that the taxon may still be extant. This is to say that 9 out of 10 of the taxon have latest extinction dates predicted to be in the future. This is shown by the codes below. 

````R
> Scrobicularia<-subset(DataPBDB,DataPBDB[,"genus"]=="Scrobicularia")
> Meiocardia<-subset(DataPBDB,DataPBDB[,"genus"]=="Meiocardia")
> Dimya<-subset(DataPBDB,DataPBDB[,"genus"]=="Dimya")
> Digitaria<-subset(DataPBDB,DataPBDB[,"genus"]=="Digitaria")
> Cuspidaria<-subset(DataPBDB,DataPBDB[,"genus"]=="Cuspidaria")
> Arctica<-subset(DataPBDB,DataPBDB[,"genus"]=="Arctica")
> Aloides<-subset(DataPBDB,DataPBDB[,"genus"]=="Aloides")
> Kurtiella<-subset(DataPBDB,DataPBDB[,"genus"]=="Kurtiella")
> Gouldia<-subset(DataPBDB,DataPBDB[,"genus"]=="Gouldia")
> Acrosterigma<-subset(DataPBDB,DataPBDB[,"genus"]=="Acrosterigma")
> estimateExtinction(Scrobicularia[,"min_ma"],0.95)
 Earliest    Latest 
  0.01170 -34.70966 
> estimateExtinction(Meiocardia[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -5.329574 
> estimateExtinction(Dimya[,"min_ma"],0.95)
 Earliest    Latest 
 0.781000 -2.054688 
> estimateExtinction(Digitaria[,"min_ma"],0.95)
 Earliest    Latest 
 0.781000 -3.761154 
> estimateExtinction(Cuspidaria[,"min_ma"],0.95)
 Earliest    Latest 
2.5880000 0.7771965 
> estimateExtinction(Arctica[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -1.799104 
> estimateExtinction(Aloides[,"min_ma"],0.95)
Earliest   Latest 
   5.333     -Inf 
> estimateExtinction(Kurtiella[,"min_ma"],0.95)
 Earliest    Latest 
  0.01170 -34.70966 
> estimateExtinction(Gouldia[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -2.047386 
> estimateExtinction(Acrosterigma[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -3.481128 
````
