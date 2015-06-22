# GCD-project
 Getting and cleaning data project
 
 The data is available in the following 8 files:
 
 1. features : for a general feature list for both test and training data
 2. activity_labels : a vector made of six possible poistion for both test and training data
 3. X_test : all the raw data regarding the Samsung for test measurements 
 4. y_test : all the activities associated with a particular observation
 5. subject_test : the test subject associated with a particular test observation
 6. X_train : all the raw data regarding the Samsung for train measurements
 7. y_train : all the activities associated with a particular train observation
 8. subject_train : the test subject associated with a particular observation
 
The process of getting down to tidy data breaks down into the following steps:

##### 1. Loading the necessary Libraries 

The two libraries data.table and "reshape2" are being used that is being load at the begining of the code. 
Loading the necessary packages
library("data.table")
library("reshape2")

##### 2. Reading & merging the Data:

###### 2.1.Reading General data:

Start by reading the general data into R. These two parameters are:

1. activity_labels: which is a factor consists of six different activity such as Laying, Standing, Walking, Walking up stairs and Walking down stairs and sitting.
2. features: 561 different measurements that has been done for both test and training observations

activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt", header = FALSE, sep = " ")[,2] 
 features <- read.table("./UCI HAR Dataset/features.txt", header = FALSE, sep = " ")[,2]
 features <- gsub("()", "", features)

###### 2.2. Reading the test data and changing the names and types

We read three variables asscoiates with test measurements:
1. x_test: includes all the test measurment data. It is a  2947 by 561 data frame that has all the 2947 test observation and 561 test measurement data.
2. y_test: Includes the activity code related to each test measurements. One of six activities that are in activity_label. It is a 2947 by 1 data frame that has one activity per test observation.
3. subject_test: the vector with all the test subjects. It is also a 2947 by 1 that has one subject per test observation in each row.

x_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
  y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
  subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")

features consists of the name of each of the 561 measurements associated with each observation. We also assign the Activity and Subject names to the relevant parameters. Subject test that is in factor format should change class to data.table to be able to be merged.

names(x_test) <- features
  names(y_test) <- "Activity" ; names(subject_test) <- "Subject"
  subject_test <- as.data.table(subject_test)
  
###### 2.3. Merging all test data:

x_test_all is the merged table resulted from merging subject_test, x_test and y_test. by merging these tables we will have the Activity and Subject associated with each observation merged with the rest of the data.

x_test_all <- cbind(subject_test, y_test, x_test)

since subject_test and y_test will each add a column to the data table, the resulted x_test_all will be a 2947 by 563 data frame. 

###### 2.5. Reading the training data and changing the names & types

We read three variables asscoiates with train measurements:
1. x_train: includes all the train measurment data. It is a  7352 by 561 data frame that has 7352 train observation and all the 561 train easurement data.
2. y_train: Includes the activity code related to each training measurements. One of six activities that are in activity_label. It is a 7352 by 1 data frame that has one activity per observation.
3. subject_train: the vector with all the train subjects. It is also a 7352 by 1 that has one subject per training observation in each row.

x_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
  y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")
  subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")

features consists of the name of each of the 561 measurements associated with each observation. We also assign the Activity and Subject names to the relevant parameters. Subject_train that is in factor format should change class to data.table to be able to be merged.

names(x_train) <- features
  names(y_train) <- "Activity" ; names(subject_train) <- "Subject"
  subject_train <- as.data.table(subject_train)

###### 2.6. Merging all trainingg data

x_train_all is the merged table resulted from merging subject_train, x_train and y_train. by merging these tables we will have the Activity and Subject associated with each observation merged with the rest of the data.

x_train_all <- cbind(subject_train, y_train, x_train)

since subject_train and y_train will each add a column to the data table, the resulted x_train_all will be a 7352 by 563 data frame.

###### 2.7. Merging test and training data

Here we merge both test and training tables resuled from lines above by rbind since they both have the same number of columns (563). We call the resulted data, all_data that has all the data subject and activity codes in it as well as the correct name os the columns. all_data is a 10299 by 563 data frame.

all_data <- rbind(x_test_all, x_train_all)

##### 3. Filter the data: 

Filter the columns with mean or standard deviation parameters.in order to do that we start by filtering the featiures that have mean or STD in it.

selected_features <- grepl("mean|std", features)

selected_features is a logical vector that marks the columns that have mean or STD measures and has 561 elements.

We add two TRUE elements to it and create resized_selected_features that has 563 elements.

  first_two_columns <- c(TRUE, TRUE)
  resized_selected_features <- append(selected_features, first_two_columns, after = 0)

Then we create selected_data which is the total data filtered by "mean" and "STD" measurements(columns). selected_data is a 10299 by 81 data frame.   
  
  selected_data <- subset(all_data, select = resized_selected_features)

##### 4. Assign activity labels

Since all the activity column has activity code, we assign the activity name by:

selected_data$Activity <- activity_labels[selected_data$Activity]

##### 5. Melt the data and cast to calculate the mean of the measurements and reshape the data

In order to calculate the mean of each measurement associated with one subject and activity, we should first melt the table with id variables, "Subject" and "Activity". and then cast it with "mean" function. melted_data is resulted from melting selected_data and is a 813621 by 4 data frame with data like:

head(melted_data, 10)
    Subject Activity          variable     value
 1:       2        5 tBodyAcc-mean()-X 0.2571778
 2:       2        5 tBodyAcc-mean()-X 0.2860267
 3:       2        5 tBodyAcc-mean()-X 0.2754848
 4:       2        5 tBodyAcc-mean()-X 0.2702982
 5:       2        5 tBodyAcc-mean()-X 0.2748330
 6:       2        5 tBodyAcc-mean()-X 0.2792199
 7:       2        5 tBodyAcc-mean()-X 0.2797459
 8:       2        5 tBodyAcc-mean()-X 0.2746005
 9:       2        5 tBodyAcc-mean()-X 0.2725287
10:       2        5 tBodyAcc-mean()-X 0.2757457

then we cast it with:

tidy_data <- dcast(melted_data, Subject ~ Activity)

and create tidy_data which is a 



  melted_data <- melt(selected_data, id.vars = c("Subject", "Activity"))##, measure.vars = selected_features)
  tidy_data <- dcast(melted_data, Subject ~ Activity)

##### 6. Write to a .txt file
