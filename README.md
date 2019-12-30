The R script, run_analysis.R, does the following:

1.Merges the training and the test sets to create one data set.
2.Extracts only the measurements on the mean and standard deviation for each measurement.
3.Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
The end result is shown in the file  menordata.txt.





# Run_Analysis
Final Project Course 3




#1.setting the file directory

setwd("C:\Users\PP\Documents\Data Science\getdata_projectfiles_UCI HAR Dataset (1).zip\UCI HAR Dataset")

#2.reading the data:

FEATURES<-read.table("features.txt",header=FALSE)
activity_labels<-read.table("activity_labels.txt",header=FALSE)
subjectrain<-read.table("subject_train.txt",header=FALSE)
xtrain<-read.table("X_train.txt",header=FALSE)
ytrain<-read.table("y_train.txt",header=FALSE)
subjectest<-read.table("subject_test.txt",header=FALSE)
xtest<-read.table("X_test.txt",header=FALSE)
ytest<-read.table("y_test.txt",header=FALSE)

#3. Colnames for the data

colnames(activity_labels)<-c("ID","Type")
colnames(subjectrain)<-c("ID2")
colnames(xtrain)<-FEATURES[,2]
colnames(ytrain)<-"ID"
colnames(subjectest)<-c("ID2")
colnames(xtest)<-FEATURES[,2]
colnames(ytest)<-"ID"

#4. Merge

datamerge1<-cbind(ytrain,subjectrain,xtrain)
datamerge2<-cbind(ytest,subjectest,xtest)
final<-rbind(datamerge1,datamerge2)

#5. Extracts only the measurements on the mean and standard deviation for each measurement.

measurements <-final[,grepl("mean|std|subject|ID",colnames(final))]

#6. Uses descriptive activity names to name the activities in the data set

measurements <- join(measurements, activity_labels, by = "ID", match = "first")
measurements <-measurements[,-1]

#7.Labeling the data set with descriptive variable names.

names(measurements) <- gsub("\\(|\\)", "", names(measurements), perl  = TRUE)
names(measurements) <- gsub("Acc", "Acceleration", names(measurements))
names(measurements) <- gsub("Freq", "Frequency", names(measurements))
names(measurements) <- gsub("Mag", "Magnitude", names(measurements))
names(measurements) <- gsub("^t", "Time", names(measurements))
names(measurements) <- gsub("^f", "Frequency", names(measurements))
names(measurements) <- gsub("BodyBody", "Body",names(measurements))
names(measurements) <- gsub("mean", "Mean", names(measurements))
names(measurements) <- gsub("std", "Standard", names(measurements))


#8.From the data set, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

menordata<-ddply(measurements,c("ID","type"),numcolwise(mean))
write.table(menordata,file="menordata.txt")
