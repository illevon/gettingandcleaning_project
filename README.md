# gettingandcleaning_project
Repository for the files of the Course Project (Coursera Course 'Getting and Cleaning Data)

### Course: Getting and Cleaning Data Coursera
Author: Anna Novelli

***
## Description
This is a ReadMe file created to fulfill the requirements of the Coursera course "Getting and Cleaning Data".
***

According to the requirements for the course project:
'You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.'

In the repository you may find:

 - the R script named 'run_analysis.R'. This script also contains the step by step documentation of the transformation applied to the source data
 - the Code book named 'CodeBook.md', which describes the data used, as well as the transformations and resulting dataset of 'run_analysis.R'
 - and this readme file named 'README.md' which lists all files in the repo. Below you may also find the R script with documentation of steps:
 
## Documentation of Rscript:

set working directory to UCI HAR Dataset folder and load all necessary libraries

`rm(list=ls())
setwd(".03data/UCI HAR Dataset")
library(data.table)
library(dplyr)
library(reshape2)`

### First Task: Merges the training and the test sets to create one data set.
load test data:

`testSubjects<-read.delim("./test/subject_test.txt", header = FALSE, sep = "", col.names = "subject")`


the 561 length feature vector:


`testFeatures<-read.delim("./test/X_test.txt", header = FALSE, sep = "")
head(testFeatures)
dim(testFeatures)`


the activity label:

`testActivity<-read.delim("./test/Y_test.txt", header = FALSE, sep = "", col.names = "activity")
head(testActivity)
dim(testActivity)`

same for train data:

`trainSubjects<-read.delim("./train/subject_train.txt", header = FALSE, sep = "", col.names = "subject")`

the 561 length feature vector:

`trainFeatures<-read.delim("./train/X_train.txt", header = FALSE, sep = "")
head(trainFeatures)
dim(trainFeatures)`

the activity label:

`trainActivity<-read.delim("./train/Y_train.txt", header = FALSE, sep = "", col.names = "activity")
head(trainActivity)
dim(trainActivity)`

get activity labels and features:

`activityLabels<-read.delim("./activity_labels.txt", header = FALSE, sep = "",  col.names = c("value", "activity"))
head(activityLabels)
features<-read.delim("./features.txt", header = FALSE, sep = "", col.names = c("index", "measure"))
head(features)`

create vector indicating whether mesure contains "mean" or "std"

`features$measure <- gsub('[()]', '', features$measure)
features$measure <- gsub('[,]', '', features$measure)
mean_sd_features <- grep("(mean|std)", features$measure)`

merge data together

since no id variables are specified, I assume that the order of observations in each dataset is meaningful. Therefore: cbind() to get test dataset and train dataset:

`testData<-cbind(testSubjects, testActivity, testFeatures)
rm(testSubjects, testActivity, testFeatures)
trainData<-cbind(trainSubjects, trainActivity, trainFeatures)
rm(trainSubjects, trainActivity, trainFeatures)
allData<-rbind(testData, trainData)
rm(testData, trainData)`

name variables of measurements according to features$measures:

`names(allData)<-c("subject", "activity", features$measure)`

labels for activity keys in are given by activityLabels$activity, set activity labels:

`allData$activity<-factor(allData$activity, levels = activityLabels$value, labels = tolower(activityLabels$activity))
rm(features, activityLabels)`

note: this actually fulfills task 3 (descriptive activity names)


### Second Task: Extracts only the measurements on the mean and standard deviation for each measurement.

this task has been prepared before by creating the vector mean_sd_feat, indicating columns, which contain mean or std in their name.since columns subject and activity have been added before the measurements in allData, mean_sd_features has to be updated before subsetting allData:

`mean_sd_features<-c(1,2,mean_sd_features+2)
mean_sd_Data<-allData[,mean_sd_features]`

### Third task: Uses descriptive activity names to name the activities in the data set
Already done during task 1

### Fourth task: Appropriately labels the data set with descriptive variable names.

The variables have been named before; still some improvements can be made:
According to the lecture, names of variables should be:

 - All lower case when possible 
 - Descriptive (Diagnosis versus Dx) 
 - Not duplicated 
 - Not have underscores or dots or white spaces
 
`names(mean_sd_Data)<-tolower(names(mean_sd_Data))`

other requirements on variable names are already fulfilled; also, variable activity is already factor; subject can be converted to factor:

`mean_sd_Data$subject<-factor(mean_sd_Data$subject)`

remove datasets the are not of use anymore

`rm(allData, mean_sd_features)`

### Fifth task: From the data set in step 4, creates a second, independent tidy data set 

with the average of each variable for each activity and each subject.

`tidyDataset<-mean_sd_Data %>%
  reshape2::melt(id=c("subject", "activity")) %>%
  reshape2::dcast(subject + activity ~ variable, fun.aggregate = mean)
write.table(tidyDataset, file="./tidyDataset.txt", row.name=FALSE)`


 
