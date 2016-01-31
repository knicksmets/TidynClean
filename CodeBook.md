###Final_Script
###Date: Jan 31, 2016
####Author:JRS
### THIS IS THE START OF CREATING ONE DATASET 
##### READING IN THE TEST DATA, SUBJECT TEST FILE
# FEATURES AND ACTIVITY LABELS
# PART 1.0 
# READING IN THE TEST DIRECTORY DATA
# READING IN THE SUBJECT DATA
# READING IN THE FEATURES DATA
# READING IN THE ACTIVITY DATA
setwd("~/2015-October/ProjectData/UCI HAR Dataset/test")
aadataXtest <- Contains the dataframe of X test data read.table("X_test.txt") --
aadataYtest <- Contains the dataframe of Y test data read.table("y_test.txt")
aaysubjectest <-Subject gathered from the test read.table("subject_test.txt")
aamydatafea<- Features List  read.table("features.txt")
activitylist<-Activities that were recoreded read.table("activity_labels.txt")
########### HERE I TRANSPOSE THE FEATURES FILE
##### THE FEATURES IN WERE A VERTICAL LIST CHANGED TO A HORIZONTAL LIST
atransposefea
<-t(aamydatafea) #### A Matrix Was Created


##### PART 1.1 READING IN THE TRAINING DATA
##### THREE FILES WERE READ IN FROM TRAINING DIRECTORY
setwd("~/2015-October/ProjectData/UCI HAR Dataset/train")
aadataXtrain<-read.table("X_train.txt")
aadataYtrain<-read.table("y_train.txt")
aaysubjectrain<-read.table("subject_train.txt")


#### PART 1.2 
##### I'M TUCKING THE TEST DATA UNDER THE TRAINING DATA
##### USING ROW BIND and storing results
axrowbind<-rbind(aadataXtrain,aadataXtest)


# I added the columames using the n tranpose process FROM THE FEATURES
# THE TRANSPOSE FILE CONTAINED A ROW THAT WAS NOT NEEDED, I SUBTRACTED IT OUT
aremovedrow<-atransposefea[-1,]####This is a charcter-string

n1<-as.data.frame(aremovedrow) #### Converted characterstring to dataframe
n2<-t(n1)
###########################
##### The features are being added
###### THIS LINE ASSIGNS VARIABLE NAMES TO COLUMNS
This variable was previously created
colnames(axrowbind)<-n2
#####################################

### PART 1.3 APPENDING THE TEST TO TRAIN DATA
#### RowBinding the Subject Files--AppendingRows #####################
##### ALSO CHANGING THE NAME OF A COLMUN FROM V1 TO SUBJECT_ID
aasubjectfilesrowbind<-rbind(aaysubjectrain,aaysubjectest)
#######bckupaasubjectfilesrowbind<-rbind(aaysubjectrain,aaysubjectest)
colnames(aasubjectfilesrowbind)[1]<-"Subject_ID"

###########RowBind-AppendingActivies######################################
######The ActivityCodes - APPENDING THE TEST TO TRAIN
aaaactivity<-rbind(aadataYtrain,aadataYtest)
# bckaaaactivity<-rbind(aadataYtrain,aaydataYtest)
colnames(aaaactivity)[1]<-"Activities"

#########THE FOLLOWING CODE WAS USED 
######TO REPLACE THE NUMBERS WITH THEIR
########NAMES IN THE ACTIVITY FILE
aaaactivity[,][aaaactivity[,]==1] <-"WALKING"
aaaactivity[,][aaaactivity[,]==2] <-"WALKING_UPSTAIRS"
aaaactivity[,][aaaactivity[,]==3] <-"WALKING_DOWNSTAIRS"
aaaactivity[,][aaaactivity[,]==4] <-"SITTING"
aaaactivity[,][aaaactivity[,]==5] <-"STANDING"
aaaactivity[,][aaaactivity[,]==6] <-"LAYING"
#####################################################################


   
#########################PART 2.0 ###############################
##################Column Bind###########################
######### Now I will do a col bind
adatasetwithact<-cbind(aaaactivity,aasubjectfilesrowbind)
asubjectwithactivities<-cbind(aasubjectfilesrowbind,aaaactivity)
###subject-then-activit-then-measures
#### SUBJECT NUMBERS, ACTIVITIES AND DATA ARE BEING MERGED HERE
a1bigmerge<-cbind(asubjectwithactivities,axrowbind)

##################END OF PHASE ONE###################################



##########IDENTIFYING THE COLUMNS That Contain Mean and Std)
columnsofconcern<-grep("mean|std",aremovedrow)
tcolumnsofconcern<-grep("mean|std",aremovedrow,value=TRUE)
##### Could Not Figure How To Automate This Section
####USING Columnnsofconcern to get the column numbers
(new<-a1bigmerge[,
                 c(1,2,3,4,5,6,7,8,43,44,45,46,47,48,83,84,85,86,87,88,123,124,125)])

colnames(new) ##### to check the names in new
afinaltidy_d_set ---> is the final dataframe only containing the needed
rows and columns
(afinaltidy_d_set<-a1bigmerge[,
                        c(1,2,3,4,5,6,7,8,43,44,45,46,47,48,83,84,85,86,87,88,123,124,125)])

##### The following is how the tidy names were assigned to each Column Heading 
names(afinaltidy_d_set)<- c("SUBJECT_ID","ACTIVITIES",
                "BdyMeanX","BdyMeanY","BdyMeanZ",
                "BdyStdX","BdyStdY",  "BdyStdX",
                "GrvMeanX", "GrvMeanY","GrvMeanZ",
                "GrvStdX","GrvStdY","GrvstdZ",
                "BJerkMeanX","BJerkMeanY","BJerkMeanZ",
                 "BJerkStdX","BJerkStdY","BJerkStdZ",
                "GyroMeanX","GyroMeanY","GyroMeanZ")

afinaltidy_d_set is the final dataframe containing the merged files containing
only the mean and standard deviations for each measurement including the subject_id and
the corresponding features. 

#########################@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@