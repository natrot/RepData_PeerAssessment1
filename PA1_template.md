# Reproducible Research: Peer Assessment 1

### Author: Jan Bezuidenhout
This report used the following libraries:

```r
library(data.table)
library(ggplot2)
```

The following report is an analysis of movement data.

The first step is to load the relevent data. 


```r
act.data <- read.csv('activity.csv')
clean.data <- na.omit(act.data) 
# str(clean.data)

clean.Data <- data.table(clean.data)
day.data <- clean.Data[, list(steps = sum(steps)), by = "date"]
```

The dataset contains 2 304 records with missing values. The method used to deal with these values was to completely ignore them.

After loading the data some summary statitistics can be used to get a better picture of the data.


```r
summary(day.data$steps)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##      41    8841   10760   10770   13290   21190
```

So the mean number of steps per day is 10 766.19 and the median number of steps are 10 765.

Next is a bar plot of the steps per day.


```r
ggplot(day.data, aes(x=date, y = steps)) + geom_bar(stat="identity") +
    theme(plot.title = element_text(lineheight=.8, face="bold"), axis.text.x = element_text(angle=90))
```

![](./PA1_template_files/figure-html/hist-1.png) 


The following plot gives the average daily steps per interval.


```r
int.data <- clean.Data[, list(steps = round(mean(steps),1)), by = "interval"]
ggplot(data=int.data, aes(x=interval, y=steps)) + geom_line() + geom_point()
```

![](./PA1_template_files/figure-html/timeseries-1.png) 

The interval during which the maximum number of steps occur, as well as the number of steps are: 835, 206.2 

The last view on the data is the difference between weekday and weekend activity.


```r
week.data <- clean.data
week.data$wd <- weekdays(as.Date(week.data$date))
week.data$week <- ifelse(week.data$wd =='Saturday' | week.data$wd =='Sunday','Weekend','Weekday')
week.data <- data.table(week.data)
week.data <- week.data[, list(steps = round(mean(steps),1)), by = "interval,week"]

ggplot(data=week.data, aes(x=interval, y=steps)) + geom_line() + geom_point() +
  facet_wrap( ~ week, nrow = 2) 
```

![](./PA1_template_files/figure-html/weekend-1.png) 

From the above plot the weekday and weekend patterns look very similar exept for and early spike in weekday data.
