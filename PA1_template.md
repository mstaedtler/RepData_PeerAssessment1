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

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

Calculate and report the mean and median total number of steps taken per day

```r
    m1 <- mean(tapply(actData$steps, actData$date, sum, na.rm = TRUE))
    m2 <- median(tapply(actData$steps, actData$date, sum, na.rm = TRUE))
```

The median is 9354.2295082 and the median is 10395.

##What is the average daily activity pattern?
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
      stepsPerInt <- aggregate(steps ~ interval, actData, mean)
      plot(stepsPerInt$steps ~ stepsPerInt$interval,
           type = "l",
           main = "Avg. daily activity pattern",
           xlab = "Interval",
           ylab = "Avg. steps")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
      max_steps <- max(stepsPerInt$steps)
      subValues <- subset(stepsPerInt, steps == max_steps)
      sub1 <- subValues$interval
```

The interval which contains the maximum number of steps is 835.

Imputing missing values

Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

Calculate and report the total number of missing values in the dataset

```r
      missingValues <- nrow(actData[!complete.cases(actData),])
```

The total Number of missing values is: 2304.

Devise a strategy for filling in all of the missing values in the dataset. Replace NA with 5min interval.
Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
      correctedData<-actData
      correctedData[is.na(correctedData[, 1]), 1]<-stepsPerInt[is.na(correctedData[, 1]),2]
```

Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
      qplot(date, weight=correctedData$steps, data=correctedData, geom="histogram")
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-1.png) 

```r
      m3 <- mean(tapply(correctedData$steps, correctedData$date, sum, na.rm = TRUE))
      m4 <- median(tapply(correctedData$steps, correctedData$date, sum, na.rm = TRUE))
```

The new median from the corrected dataset is 9530.7244046 and the median is 1.0439 &times; 10<sup>4</sup>.
There are small changes for the mean and median values.
