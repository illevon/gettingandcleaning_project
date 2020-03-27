## Course Project 
### Course: Getting and Cleaning Data Coursera
Author: Anna Novelli

***
# CodeBook Description
This codebook describes the variables, the data, and any transformations or work that were performed to clean up the data used in the course project of the Coursera course 'Getting and Cleaning Data'.
It refers to the the Rscript 'run_analysis.R'

***
## The Data
### Source of the data
The source data set is the "Human Activity Recognition Using Smartphones Dataset
Version 1.0" and  was downloaded from here:  
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
The data represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:  
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

### Data collection:
The data collection description is taken from the README file of the Source Data: 
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 
The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.
### Information on variables:
The information on variables is taken from the README file of the Source Data:   

For each record in the dataset it is provided: 

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration. 
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

### Included datasets
Datasets and files from the data source that were used, include:

- 'features.txt': List of all features (used as column names in x_train/y_train)
- 'activity_labels.txt': (used for labeling the activity variable)
- 'x_train.txt': Training set.
- 'y_train.txt': Training labels.
- 'x_test.txt': Test set.
- 'y_test.txt': Test labels.
- 'subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
- 'subject_test.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

***
## The R script 'run_Analysis.R'
### Transformations made in 'run_analysis.R'
The transformations made in 'run_analysis.R' are the transformation necessary to fulfill the projects tasks:

1. Merge the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

More information on the specific information may be found in the comments of the R script.

### Resulting datasets
Two datasets are the final result of the R script:

- Dataset "mean_sd_Data" : 10299 Observables representing single measurements of test and training subjects. 81 variables: 'subject' indicates the subject whose movement has been measured, 'activity' indicates the activity during which the measurement was performed and the remaining 79 variables are measurements on the mean and standard deviation for each measurement . 
- Dataset "tidyDataset" : based on "mean_sd_Data"; contains the average of each measurement variable for each activity and each subject. 180 observables and 81 variables.
