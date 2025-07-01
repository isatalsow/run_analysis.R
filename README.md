# run_analysis.R

setwd("C:/Users/dell/Desktop/Course_Coursera_Datascience_Foundation UsingR_Full course/Getting and Cleaning Data(3)/Course Project/UCI HAR Dataset")


read.table("activity_labels.txt")


features <- read.table
activities <- read.table
subject_test <- read.table
x_test <- read.table
y_test <- read.table
subject_train <- read.table
x_train <- read.table
y_train <- read.table




features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")


X <- rbind(x_train, x_test)




head("UCI HAR Dataset/features.txt")
summary("UCI HAR Dataset/activity_labels.txt")
tail("UCI HAR Dataset/test/X_test.txt")
view("C:/Users/dell/Desktop/Course_Coursera_Datascience_Foundation UsingR_Full course/Getting and Cleaning Data(3)/Course Project/UCI HAR Dataset")





1.Merging training and the test sets to create one data set.


x_train <- read.table("./data/UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("./data/UCI HAR Dataset/train/y_train.txt")
subject_train <- read.table("./data/UCI HAR Dataset/train/subject_train.txt")


Reading testing tables

x_test <- read.table("./data/UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("./data/UCI HAR Dataset/test/y_test.txt")
subject_test <- read.table("./data/UCI HAR Dataset/test/subject_test.txt")

feature vector
features <- read.table('./data/UCI HAR Dataset/features.txt')

ctivity labels
activityLabels = read.table('./data/UCI HAR Dataset/activity_labels.txt')

feature vector
features <- read.table('./data/UCI HAR Dataset/features.txt')

Reading activity labels
activityLabels = read.table('./data/UCI HAR Dataset/activity_labels.txt')


Assigning column names

colnames(x_train) <- features[,2]
colnames(y_train) <-"activityId"
colnames(subject_train) <- "subjectId"

colnames(x_test) <- features[,2] 
colnames(y_test) <- "activityId"
colnames(subject_test) <- "subjectId"

colnames(activityLabels) <- c('activityId','activityType')

Merging all data in one set

mrg_train <- cbind(y_train, subject_train, x_train)
mrg_test <- cbind(y_test, subject_test, x_test)
setAllInOne <- rbind(mrg_train, mrg_test)

dim(setAllInOne)
[1] 10299   563


Extracts only the measurements on the mean and standard deviation for each measurement
Reading column names
  
colNames <- colnames(setAllInOne)

Create vector defining ID, mean and standard deviation:

  mean_and_std <- (grepl("activityId" , colNames) | grepl("subjectId" , colNames) | 
grepl("mean.." , colNames) | grepl("std.." , colNames) 
  )

subset from setAllInOne
setForMeanAndStd <- setAllInOne[ , mean_and_std == TRUE]


Step 3. Uses descriptive activity names to name the activities in the data set

setwithactivityNames <- merge(setForMeanAndStd, activityLabels,by='activityId', all.x=TRUE)

Step 4. Appropriately labels the data set with descriptive variable names

Done in steps, see 1.3,2.2 and 2.3

5: From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject

Make a second tidy data set

secTidySet <- aggregate(. ~subjectId + activityId, setWithActivityNames, mean)
secTidySet <- secTidySet[order(secTidySet$subjectId, secTidySet$activityId),]

5.2 Writing second tidy data set in txt file
write.table(secTidySet, "secTidySet.txt", row.name=FALSE)
