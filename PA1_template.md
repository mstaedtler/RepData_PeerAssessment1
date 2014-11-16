#Reproducible Research - Peer Assessment 1

##Loading and preprocessing the data

Show any code that is needed to Load the data (i.e. read.csv()) and to transform the data and showing the summary.





```r
      file <- "~/WorkspaceR/05_ReproducibleResearch/Peer Assessment 1/activity.csv"
      actData <- read.csv(file = file, sep = ",")
      summary(actData)
```

```
##      steps                date          interval     
##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
##  Median :  0.00   2012-10-03:  288   Median :1177.5  
##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2  
##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
##  NA's   :2304     (Other)   :15840
```
##What is mean total number of steps taken per day?
Make a histogram of the total number of steps taken each day

```r
      library(ggplot2)
      q<-qplot(date, weight=actData$steps, data=actData, geom="histogram")
      print(q)
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

Calculate and report the mean and median total number of steps taken per day

```r
    m1 <- mean(tapply(actData$steps, actData$date, sum, na.rm = TRUE))
    m2 <- median(tapply(actData$steps, actData$date, sum, na.rm = TRUE))
```

The median is 9354.2295082 and the median is 10395.

##What is the average daily activity pattern?
