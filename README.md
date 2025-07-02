# run_analysis.R

setwd("C:/Users/dell/Desktop/Course_Coursera_Datascience_Foundation UsingR_Full course/Getting and Cleaning Data(3)/Course Project/UCI HAR Dataset")

# Load libraries
library(dplyr)

# Step 1: Read data
features <- read.table("UCI HAR Dataset/features.txt")
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$V2)
y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")
subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$V2)
y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")

# Step 2: Merge training and test sets
X <- rbind(x_train, x_test)
Y <- rbind(y_train, y_test)
Subject <- rbind(subject_train, subject_test)
merged <- cbind(Subject, Y, X)

# Step 3: Extract mean and std measurements
tidy <- merged %>% select(subject, code, contains("mean"), contains("std"))

# Step 4: Use descriptive activity names
tidy$code <- activities[tidy$code, 2]

# Step 5: Label with descriptive names
names(tidy)[2] <- "activity"
names(tidy) <- gsub("Acc", "Accelerometer", names(tidy))
names(tidy) <- gsub("Gyro", "Gyroscope", names(tidy))
names(tidy) <- gsub("BodyBody", "Body", names(tidy))
names(tidy) <- gsub("^t", "Time", names(tidy))
names(tidy) <- gsub("^f", "Frequency", names(tidy))

# Step 6: Create independent tidy dataset
final_data <- tidy %>% group_by(subject, activity) %>% summarise_all(mean)
write.table(final_data, "Final_Tidy_Data.txt", row.name = FALSE)
