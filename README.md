# GCD-project
 Getting and cleaning data project
 
 The data is available in the following 8 files:
 
 1. features : for a general feature list for both test and training data
 2. activity_labels : a vector made of six possible poistion for both test and training data
 3. X_test : all the raw data regarding the Samsung for test measurements 
 4. y_test : 
 5. subject_test :
 6. X_train :
 7. y_train :
 8. subject_train :
 
 
The process of getting down to tidy data breaks down into the following steps:

##### 1. Loading the necessary Libraries 
The two libraries "data.table" and "reshape2" are being used that is being load at the begining of the code.
'''
Loading the necessary packages
library("data.table")
library("reshape2")
'''
##### 2. Reading & merging the Data:

'''
 activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt", header = FALSE, sep = " ")[,2] 
 features <- read.table("./UCI HAR Dataset/features.txt", header = FALSE, sep = " ")[,2]
 features <- gsub("()", "", features)
 '''
###### 2.1.Reading General data
###### 2.2. Reading the test data and changing the names and types
###### 2.3. Merging all test data
###### 2.4. Reading the training data and changing the names & types
###### 2.5. Merging all trainingg data
###### 2.6. Merging test and training data
##### 3. Filter the data: Filter the columns with mean or standard deviation
##### 4. Assign activity labels
##### 5. Melt the data and cast to calculate the mean of the measurements and reshape the data
##### 6. Write to a .txt file
