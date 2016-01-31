The Script: ProjectGettingNCleanCode_Jan_31-Week3.R
Meets Requirements 1,2,3,4 but NOT 5.
Author:JRS
Date: Jan 31, 2016 
final dataframe name: afinaltidy_d_set
text data set created from final df: final_tidy_dataset


###Final_Script
###Date: Jan 31, 2016
### THIS IS THE START OF CREATING ONE DATASET 
##### READING IN THE TEST DATA, SUBJECT TEST FILE
# FEATURES AND ACTIVITY LABELS
# PART 1.0 
# READING IN THE TEST DIRECTORY DATA
# READING IN THE SUBJECT DATA
# READING IN THE FEATURES DATA
# READING IN THE ACTIVITY DATA

########### HERE I TRANSPOSE THE FEATURES FILE
##### THE FEATURES IN WERE A VERTICAL LIST CHANGED TO A HORIZONTAL LIST

##############FOR CONVERSION IF NEEDED###############################
#forconver<-read.table("y_test.txt")
####################DIMENSION CHECK################################
#dim(amydatafea)
#class(amydatafea)
#dim(atransposefea)
#class(atransposefea) ### 
#################atransposefea is a matrix###########

##### PART 1.1 READING IN THE TRAINING DATA
##### THREE FILES WERE READ IN FROM TRAINING DIRECTORY
setwd("~/2015-October/ProjectData/UCI HAR Dataset/train")
aadataXtrain<-read.table("X_train.txt")
aadataYtrain<-read.table("y_train.txt")
aaysubjectrain<-read.table("subject_train.txt")


#### PART 1.2 
##### I'M TUCKING THE TEST DATA UNDER THE TRAINING DATA
##### USING ROW BIND

axrowbind<-rbind(aadataXtrain,aadataXtest)
#  keepaxrowbind<-rbind(aadataXtrain,aadataXtest) ### Backup copy of rowbind

# I added the columames using the n tranpose process FROM THE FEATURES
# THE TRANSPOSE FILE CONTAINED A ROW THAT WAS NOT NEEDED, I SUBTRACTED IT OUT
aremovedrow<-atransposefea[-1,]####This is a charcter-string
n1<-as.data.frame(aremovedrow) #### Converted characterstring to dataframe
n2<-t(n1)
###########################

###### THIS LINE ASSIGNS VARIABLE NAMES TO COLUMNS
colnames(axrowbind)<-n2
#####################################

### PART 1.3 APPENDING THE TEST TO TRAIN DATA
#### RowBinding the Subject Files--AppendingRows #####################
##### ALSO CHANGING THE NAME OF A COLMUN FROM V1 TO SUBJECT_ID
#######bckupaasubjectfilesrowbind<-rbind(aaysubjectrain,aaysubjectest)


###########RowBind-AppendingActivies######################################
######The ActivityCodes - APPENDING THE TEST TO TRAIN
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
a1bigmerge<-cbind(asubjectwithactivities,axrowbind)


#nfram<-data.frame(a1thefinaldataset[,1])


##################END OF PHASE ONE###################################

#library(dplyr)
#testselect<-select(thefinaldataset,"*mean")
#head(select(thefinaldataset,Subject))


##########IDENTIFYING THE COLUMNS That Contain Mean and Std)
columnsofconcern<-grep("mean|std",aremovedrow)
tcolumnsofconcern<-grep("mean|std",aremovedrow,value=TRUE)
##### Could Not Figure How To Automate This Section
####USING Columnnsofconcern to get the column numbers
(new<-a1bigmerge[,
                 c(1,2,3,4,5,6,7,8,43,44,45,46,47,48,83,84,85,86,87,88,123,124,125)])

colnames(new) ##### to check the names in new

(afinaltidy_d_set<-a1bigmerge[,
                        c(1,2,3,4,5,6,7,8,43,44,45,46,47,48,83,84,85,86,87,88,123,124,125)])

#### THIS IS USED TO CHECK AND VERIFY THE COLUMNAMES
#cn<-c(colnames(new))
#cn[3:8]
#cn[9:14]
#cn[15:20]
#cn[21:23]
##############################
### USED TO ASSIGN THE NEW AND IMPROVED COLUMNAMES 
names(afinaltidy_d_set)<- c("SUBJECT_ID","ACTIVITIES",
                "BdyMeanX","BdyMeanY","BdyMeanZ",
                "BdyStdX","BdyStdY",  "BdyStdX",
                "GrvMeanX", "GrvMeanY","GrvMeanZ",
                "GrvStdX","GrvStdY","GrvstdZ",
                "BJerkMeanX","BJerkMeanY","BJerkMeanZ",
                 "BJerkStdX","BJerkStdY","BJerkStdZ",
                "GyroMeanX","GyroMeanY","GyroMeanZ")

#importlist<-c("SUBJECT_ID","ACTIVITIES",
#              "BdyMeanX","BdyMeanY","BdyMeanZ",
#              "BdyStdX","BdyStdY",  "BdyStdX",
#              "GrvMeanX", "GrvMeanY","GrvMeanZ",
#              "GrvStdX","GrvStdY","GrvstdZ",
#              "BJerkMeanX","BJerkMeanY","BJerkMeanZ",
#              "BJerkStdX","BJerkStdY","BJerkStdZ",
#              "GyroMeanX","GyroMeanY","GyroMeanZ")

####################################################################
#Finalcolumnsofconcern<-grep("mean|std",thefinaldataset)
#tFinalcolumnsofconcern<-grep("mean|std",thefinaldataset,value=TRUE)
#################################################################



#########################@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@



We have the test directory which contain three files
a)  	X-test   	b) Y-test  and   c) subject_test 
We have a features.txt file  containing  the following  e.g �tBodyAcc-mean(), tBodyAcc-std()-Z� the file contains  561 rows. We only need std/mean.	
The activity_labels, there are only six activities ( Walking,  Walking_Upstairs,  Walking_Downstairs, Sitting, Standing, Laying ) 
The experiment was carried out with 30 volunteers.  Each person performed six activities. The activities are coded. 
1.   MainSourceFile: ProjectCode � I am appending  the �test data� to the �training data�.  The training files have more rows than the testing files.	
 
1)  	Directories) �  Created Two Directories on my laptop.  Read the using the read.table function.
2)  	UCI HAR Dataset/train
3)  	UCI HAR Dataset/Test ---- The y-test & y-train contains the id�s of the participants. The first column is an ID-column.
 
Rows
Columns
 
Subject Test
2947
1
 
Subject Train
7372
1
 
X -  	Test
2947
561
 
X - Train
7352
561
 
Y-    	Test
2947
1
Contain the activity code.  (1-6)
Y �  Train
7352
1
Contain the activity code. (1-6)
1>	 
The training files have more rows than the testing files.
 
2>	Subjects --- Just the individuals that participated in the training and testing
There is a subject file for both testing and training--
Features --- The various types of measurements that could be made
X-Parameter --- For Training
X-Parameter --- For Testing
Y-Parameter  --- For Training
Y-Parameter --- For  Testing
(X-TrainingTheModel, Y-TrainingTheModel) � The combination of the two components are identified with one subject
3>	The activity file must be used  for both the y-test/y-train file
4>	The features needed to be reshaped
Needed to cast/melt  the data --- Added the reshape library
 
5>	The feature file is a data.frame before using the �t� function
a)  	561 rows ---- two columns
The t function creates two rows and 561 columns.
I could rbind the transposed data.frame. Frame was converted into a matrix
Transpose the feature file
The �t� function converted the dataframe to a matrix.
IV) � I first created a dataframe containing the X-Train and X-Test data only using the rbind function
V)  Massaging the �feature file�
a) subtracted out the first row using �atransposefea[-1,] which created a vector/array of characters titles
 
character----�data.frame-----�matrix
 
the matrix was used to change the column names
 

