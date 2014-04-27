UCI-HAR-Assignment
==================


```{r}
# read all the feature(of measurements) and activity(WALKING, SITTING...)
feature <- read.table("./data/UCI HAR Dataset/features.txt")
activity <- read.table("./data/UCI HAR Dataset/activity_labels.txt")

# read training data set, activity and subject
X_train <- read.table("./data/UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("./data/UCI HAR Dataset/train/y_train.txt")
subject_train <- read.table("./data/UCI HAR Dataset/train/subject_train.txt")

# combine subject and activity as a separate column for each row
X_train <- cbind(subject_train, y_train, X_train)

# set the column name as descriptive names
colnames(X_train) <- c("subject", "activity", as.character(feature$V2))

# change the activity number to descriptive activity name
X_train$activity <- factor(X_train$activity, labels=activity$V2)

# read test data set, activity and subject
X_test <- read.table("./data/UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("./data/UCI HAR Dataset/test/y_test.txt")
subject_test <- read.table("./data/UCI HAR Dataset/test/subject_test.txt")

# combine subject and activity as a separate column for each row
X_test <- cbind(subject_test, y_test, X_test)

# combine subject and activity as a separate column for each row
colnames(X_test) <- c("subject", "activity", as.character(feature$V2))

# change the activity number to descriptive activity name
X_test$activity <- factor(X_test$activity, labels=activity$V2)

# merge the two datasets
X_fullset <- rbind(X_train, X_test)

# extract only the mean and std of measure
X_mean_std <- X_fullset[, c(1, 2, grep("mean[(]|std[(]", colnames(X_fullset)))]

# create new data set of mean for each subject and each activity
X_tidy_mean <- aggregate(.~subject+activity, data=X_mean_std, mean)

# save it to local file
write.csv(X_tidy_mean, "./data/UCI_tidy.csv")

```

