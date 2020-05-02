Introduction
------------

It is now possible to collect a large amount of data about personal
movement using activity monitoring devices such as a Fitbit, Nike
Fuelband, or Jawbone Up. These type of devices are part of the
“quantified self” movement – a group of enthusiasts who take
measurements about themselves regularly to improve their health, to find
patterns in their behavior, or because they are tech geeks. But these
data remain under-utilized both because the raw data are hard to obtain
and there is a lack of statistical methods and software for processing
and interpreting the data.

This assignment makes use of data from a personal activity monitoring
device. This device collects data at 5 minute intervals through out the
day. The data consists of two months of data from an anonymous
individual collected during the months of October and November, 2012 and
include the number of steps taken in 5 minute intervals each day.

The data for this assignment can be downloaded from the course web site:

    Dataset: Activity monitoring data [52K]

The variables included in this dataset are:

    steps: Number of steps taking in a 5-minute interval (missing values are coded as NA\color{red}{\verb|NA|}NA)
    date: The date on which the measurement was taken in YYYY-MM-DD format
    interval: Identifier for the 5-minute interval in which measurement was taken

The dataset is stored in a comma-separated-value (CSV) file and there
are a total of 17,568 observations in this dataset.

Loading and preprocessing the data
----------------------------------

Download data and unzip it into the pre-assigned directory

Load the data and check the structure of the dataset

    activity <- read.csv("activity.csv")
    str(activity)

    ## 'data.frame':    17568 obs. of  3 variables:
    ##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...

What is mean total number of steps taken per day?
-------------------------------------------------

1.Calculate the total number of steps taken per day - NA values excluded

    daysteps <- aggregate(steps ~ date, activity, sum, na.rm = TRUE )
    daysteps

    ##          date steps
    ## 1  2012-10-02   126
    ## 2  2012-10-03 11352
    ## 3  2012-10-04 12116
    ## 4  2012-10-05 13294
    ## 5  2012-10-06 15420
    ## 6  2012-10-07 11015
    ## 7  2012-10-09 12811
    ## 8  2012-10-10  9900
    ## 9  2012-10-11 10304
    ## 10 2012-10-12 17382
    ## 11 2012-10-13 12426
    ## 12 2012-10-14 15098
    ## 13 2012-10-15 10139
    ## 14 2012-10-16 15084
    ## 15 2012-10-17 13452
    ## 16 2012-10-18 10056
    ## 17 2012-10-19 11829
    ## 18 2012-10-20 10395
    ## 19 2012-10-21  8821
    ## 20 2012-10-22 13460
    ## 21 2012-10-23  8918
    ## 22 2012-10-24  8355
    ## 23 2012-10-25  2492
    ## 24 2012-10-26  6778
    ## 25 2012-10-27 10119
    ## 26 2012-10-28 11458
    ## 27 2012-10-29  5018
    ## 28 2012-10-30  9819
    ## 29 2012-10-31 15414
    ## 30 2012-11-02 10600
    ## 31 2012-11-03 10571
    ## 32 2012-11-05 10439
    ## 33 2012-11-06  8334
    ## 34 2012-11-07 12883
    ## 35 2012-11-08  3219
    ## 36 2012-11-11 12608
    ## 37 2012-11-12 10765
    ## 38 2012-11-13  7336
    ## 39 2012-11-15    41
    ## 40 2012-11-16  5441
    ## 41 2012-11-17 14339
    ## 42 2012-11-18 15110
    ## 43 2012-11-19  8841
    ## 44 2012-11-20  4472
    ## 45 2012-11-21 12787
    ## 46 2012-11-22 20427
    ## 47 2012-11-23 21194
    ## 48 2012-11-24 14478
    ## 49 2012-11-25 11834
    ## 50 2012-11-26 11162
    ## 51 2012-11-27 13646
    ## 52 2012-11-28 10183
    ## 53 2012-11-29  7047

2.If you do not understand the difference between a histogram and a
barplot, research the difference between them. Make a histogram of the
total number of steps taken each day

    hist(daysteps$steps)
    
https://github.com/AndreG86600/Reproducible-Research-Week2/blob/master/Avg_Steps_Int.png

3.Calculate and report the mean and median of the total number of steps
taken per day

    mean_steps <- mean(daysteps$steps)
    mean_steps

    ## [1] 10766.19

    median_steps <- median(daysteps$steps)
    median_steps

    ## [1] 10765

What is the average daily activity pattern?
-------------------------------------------

1.Make a time series plot (i.e. type = “l”type=“l”) of the 5-minute
interval (x-axis) and the average number of steps taken, averaged across
all days (y-axis)

    intervalsteps <- aggregate(steps ~ interval, activity, mean, na.rm = TRUE)
    plot(steps ~ interval, data = intervalsteps, type= "l")

![](Assignment1_files/figure-markdown_strict/unnamed-chunk-5-1.png)

2.Which 5-minute interval, on average across all the days in the
dataset, contains the maximum number of steps?

    maxsteps <- intervalsteps[which.max(intervalsteps$steps), ]$interval
    maxsteps

    ## [1] 835

Imputing missing values
-----------------------

1.Calculate and report the total number of missing values in the dataset
(i.e. the total number of rows with NANAs)

    missing_na <- sum(is.na(activity$steps))
    missing_na

    ## [1] 2304

2.Devise a strategy for filling in all of the missing values in the
dataset. The strategy does not need to be sophisticated. In this case
I´ve used the mean fo the day. Create a new dataset that is equal to the
original dataset but with the missing data filled in - I´ve kept the
same name and just overwritten the “activity” dataset.

    avgsteps <- with(activity, tapply(steps, date, mean, na.rm = TRUE))
    for (i in length(which(is.na(activity$steps)))) {
            activity[which(is.na(activity$steps)), 1] <- mean(avgsteps, na.rm=TRUE)
    }

3a. Make a histogram of the total number of steps taken each day. What
is the impact of imputing missing data on the estimates of the total
daily number of steps?

    newday_steps <- aggregate(steps ~ date, activity, sum, na.rm = TRUE )
    hist(newday_steps$steps)

![](Assignment1_files/figure-markdown_strict/unnamed-chunk-9-1.png)

3b.Calculate and report the mean and median total number of steps taken
per day. Do these values differ from the estimates from the first part
of the assignment?

    new_mean_steps <- mean(newday_steps$steps)
    new_mean_steps

    ## [1] 10766.19

    new_median_steps <- median(newday_steps$steps)
    new_median_steps

    ## [1] 10766.19

So after we´ve filled the NA values with the mean of the day, median and
mean of the “updated” dataset do not differ substantially from the
original one.

Are there differences in activity patterns between weekdays and weekends?
-------------------------------------------------------------------------

1.Create a new factor variable in the dataset with two levels –
“weekday” and “weekend” indicating whether a given date is a weekday or
weekend day.

    activity$day <- ifelse(weekdays(as.Date(activity$date)) == "Samstag" | weekdays(as.Date(activity$date)) == "Sonntag", "weekend", "weekday")

2.Make a panel plot containing a time series plot (i.e. type =
“l”type=“l”) of the 5-minute interval (x-axis) and the average number of
steps taken, averaged across all weekday days or weekend days (y-axis).
See the README file in the GitHub repository to see an example of what
this plot should look like using simulated data.

    #Calculate avg steps per weekend
    avgsteps_wnd <- tapply(activity[activity$day=="weekend",]$steps, activity[activity$day=="weekend",]$interval, mean, na.rm=TRUE)

    #Calculate avg steps per weekday
    avgsteps_wd <- tapply(activity[activity$day=="weekday",]$steps,activity[activity$day=="weekday",]$interval, mean, na.rm=TRUE)

    #Plot
    par(mfrow=c(1,2))

    plot(as.numeric(names(avgsteps_wnd)), avgsteps_wnd, xlab="Interval", ylab="Steps", main = "Avg Steps per Interval (weekend)", type="l")
    plot(as.numeric(names(avgsteps_wd)), avgsteps_wd, xlab="Interval", ylab="Steps", main = "Avg Steps per Interval (weekday)", type="l")

![](Assignment1_files/figure-markdown_strict/unnamed-chunk-12-1.png)
