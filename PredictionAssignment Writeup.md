                                      Week4 : Prediction Assignment Write up


The goal of this work is to predict the activity done by humans based on the analysis done in http://groupware.les.inf.puc-rio.br/har.  To build this model, we are using pml training and testing datasets, which is being provided in the above mentioned website.
PACKAGES USED FOR ANALYSIS
For the analysis , the following packages are loaded
> library(ggplot2)
> library(scales)
> library(dplyr)
> library(caret)

READ THE DATA:
Initially we have to read the csv files pml-training  and pml-testing and also have to identify the missing values ,NA , Divison error values
> pmltrain<-read.csv("E:/Rprogram/mlweek4/pml-training.csv",header=T,na.strings=c("NA","","#DIV/0!"))
> dim(pmltrain)
[1] 19622   160
> colnames(pmltrain)
  [1] "X"                        "user_name"               
  [3] "raw_timestamp_part_1"     "raw_timestamp_part_2"    
  [5] "cvtd_timestamp"           "new_window"              
  [7] "num_window"               "roll_belt"               
  [9] "pitch_belt"               "yaw_belt"                
 [11] "total_accel_belt"         "kurtosis_roll_belt"      
 [13] "kurtosis_picth_belt"      "kurtosis_yaw_belt"       
 [15] "skewness_roll_belt"       "skewness_roll_belt.1"    
 [17] "skewness_yaw_belt"        "max_roll_belt"           
 [19] "max_picth_belt"           "max_yaw_belt"            
 [21] "min_roll_belt"            "min_pitch_belt"          
 [23] "min_yaw_belt"             "amplitude_roll_belt"     
 [25] "amplitude_pitch_belt"     "amplitude_yaw_belt"      
 [27] "var_total_accel_belt"     "avg_roll_belt"           
 [29] "stddev_roll_belt"         "var_roll_belt"           
 [31] "avg_pitch_belt"           "stddev_pitch_belt"       
 [33] "var_pitch_belt"           "avg_yaw_belt"            
 [35] "stddev_yaw_belt"          "var_yaw_belt"            
 [37] "gyros_belt_x"             "gyros_belt_y"            
 [39] "gyros_belt_z"             "accel_belt_x"            
 [41] "accel_belt_y"             "accel_belt_z"            
 [43] "magnet_belt_x"            "magnet_belt_y"           
 [45] "magnet_belt_z"            "roll_arm"                
 [47] "pitch_arm"                "yaw_arm"                 
 [49] "total_accel_arm"          "var_accel_arm"           
 [51] "avg_roll_arm"             "stddev_roll_arm"         
 [53] "var_roll_arm"             "avg_pitch_arm"           
 [55] "stddev_pitch_arm"         "var_pitch_arm"           
 [57] "avg_yaw_arm"              "stddev_yaw_arm"          
 [59] "var_yaw_arm"              "gyros_arm_x"             
 [61] "gyros_arm_y"              "gyros_arm_z"             
 [63] "accel_arm_x"              "accel_arm_y"             
 [65] "accel_arm_z"              "magnet_arm_x"            
 [67] "magnet_arm_y"             "magnet_arm_z"            
 [69] "kurtosis_roll_arm"        "kurtosis_picth_arm"      
 [71] "kurtosis_yaw_arm"         "skewness_roll_arm"       
 [73] "skewness_pitch_arm"       "skewness_yaw_arm"        
 [75] "max_roll_arm"             "max_picth_arm"           
 [77] "max_yaw_arm"              "min_roll_arm"            
 [79] "min_pitch_arm"            "min_yaw_arm"             
 [81] "amplitude_roll_arm"       "amplitude_pitch_arm"     
 [83] "amplitude_yaw_arm"        "roll_dumbbell"           
 [85] "pitch_dumbbell"           "yaw_dumbbell"            
 [87] "kurtosis_roll_dumbbell"   "kurtosis_picth_dumbbell" 
 [89] "kurtosis_yaw_dumbbell"    "skewness_roll_dumbbell"  
 [91] "skewness_pitch_dumbbell"  "skewness_yaw_dumbbell"   
 [93] "max_roll_dumbbell"        "max_picth_dumbbell"      
 [95] "max_yaw_dumbbell"         "min_roll_dumbbell"       
 [97] "min_pitch_dumbbell"       "min_yaw_dumbbell"        
 [99] "amplitude_roll_dumbbell"  "amplitude_pitch_dumbbell"
[101] "amplitude_yaw_dumbbell"   "total_accel_dumbbell"    
[103] "var_accel_dumbbell"       "avg_roll_dumbbell"       
[105] "stddev_roll_dumbbell"     "var_roll_dumbbell"       
[107] "avg_pitch_dumbbell"       "stddev_pitch_dumbbell"   
[109] "var_pitch_dumbbell"       "avg_yaw_dumbbell"        
[111] "stddev_yaw_dumbbell"      "var_yaw_dumbbell"        
[113] "gyros_dumbbell_x"         "gyros_dumbbell_y"        
[115] "gyros_dumbbell_z"         "accel_dumbbell_x"        
[117] "accel_dumbbell_y"         "accel_dumbbell_z"        
[119] "magnet_dumbbell_x"        "magnet_dumbbell_y"       
[121] "magnet_dumbbell_z"        "roll_forearm"            
[123] "pitch_forearm"            "yaw_forearm"             
[125] "kurtosis_roll_forearm"    "kurtosis_picth_forearm"  
[127] "kurtosis_yaw_forearm"     "skewness_roll_forearm"   
[129] "skewness_pitch_forearm"   "skewness_yaw_forearm"    
[131] "max_roll_forearm"         "max_picth_forearm"       
[133] "max_yaw_forearm"          "min_roll_forearm"        
[135] "min_pitch_forearm"        "min_yaw_forearm"         
[137] "amplitude_roll_forearm"   "amplitude_pitch_forearm" 
[139] "amplitude_yaw_forearm"    "total_accel_forearm"     
[141] "var_accel_forearm"        "avg_roll_forearm"        
[143] "stddev_roll_forearm"      "var_roll_forearm"        
[145] "avg_pitch_forearm"        "stddev_pitch_forearm"    
[147] "var_pitch_forearm"        "avg_yaw_forearm"         
[149] "stddev_yaw_forearm"       "var_yaw_forearm"         
[151] "gyros_forearm_x"          "gyros_forearm_y"         
[153] "gyros_forearm_z"          "accel_forearm_x"         
[155] "accel_forearm_y"          "accel_forearm_z"         
[157] "magnet_forearm_x"         "magnet_forearm_y"        
[159] "magnet_forearm_z"         "classe"

>pmltest<-read.csv("E:/Rprogram/mlweek4/pml-testing.csv",na.strings=c("NA","","#DIV/0!"))
SPLITTING THE TRAINING DATA:
To build the model and to create out of sample error data partition is done on pml-training data in a ration 70%:30% 
> set.seed(32234)
> inTrain<-createDataPartition(y=pmltrain$classe,list=F,p=0.7)
> trainData<-pmltrain[inTrain,]
> testData<-pmltrain[-inTrain,]
> table(is.na(trainData))

Find which variables (if any) that are mostly na values:
> naprops <- colSums(is.na(trainData))/nrow(trainData)
> mostlyNAs <- names(naprops[naprops > 0.75]) # mostly being 75%
> mostlyNACols <- which(naprops > 0.75) # there's about 100 of them

Taking a small sample data for training where all na’s are removed  
> set.seed(1256)
> smalltrain <- trainData %>% tbl_df %>% sample_n(size=1000)
> smalltrain <- smalltrain[,-mostlyNACols]
> smalltrain <- smalltrain[,-grep("X|user_name",names(smalltrain))]
> smalltrain <- smalltrain[,-grep("cvtd_timestamp",names(smalltrain))]
> smalltrain <- smalltrain[,-nearZeroVar(smalltrain)]
> modelVars <- names(smalltrain)
> modelVars1 <- modelVars[-grep("classe",modelVars)] # remove the classe var

Using random forest  with the predictors in the model variable we are predicting the classe variable
> set.seed(57)
> cleanedTrainData <- trainData[,modelVars]
> modelFit <- randomForest(classe ~., data=cleanedTrainData, type="class")

The values of the predictive model is stored in predTrain and confusion matrix is formed from predicted data to obtain the sample error
> predTrain <- predict(modelFit,newdata=trainData)
 
> confusionMatrix(predTrain,trainData$classe)$table

Reference
Prediction    A    B    C    D    E
         A 3906    0    0    0    0
         B    0 2658    0    0    0
         C    0    0 2396    0    0
         D    0    0    0 2252    0
         E    0    0    0    0 2525

To get an out sample error from the 30% pml-training data which is given as testing data
> classe_col <- grep("classe",names(testData))
> predTest <- predict(modelFit, newdata = testData[,-classe_col], type="class")
> 
> confusionMatrix(predTest,testData$classe)
Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1672    3    0    0    0
         B    2 1135    5    0    0
         C    0    1 1021    1    0
         D    0    0    0  963    1
         E    0    0    0    0 1081

Overall Statistics
                                          
               Accuracy : 0.9978          
                 95% CI : (0.9962, 0.9988)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.9972          
 Mcnemar's Test P-Value : NA              

Statistics by Class:

                     Class: A Class: B Class: C Class: D Class: E
Sensitivity            0.9988   0.9965   0.9951   0.9990   0.9991
Specificity            0.9993   0.9985   0.9996   0.9998   1.0000
Pos Pred Value         0.9982   0.9939   0.9980   0.9990   1.0000
Neg Pred Value         0.9995   0.9992   0.9990   0.9998   0.9998
Prevalence             0.2845   0.1935   0.1743   0.1638   0.1839
Detection Rate         0.2841   0.1929   0.1735   0.1636   0.1837
Detection Prevalence   0.2846   0.1941   0.1738   0.1638   0.1837
Balanced Accuracy      0.9990   0.9975   0.9974   0.9994   0.9995
Predicting exercise activity using the model
Perform the prediction
> predpmltest <- predict(modelFit, newdata = pmltest, type="class")
> print(predpmltest)
1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
 B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
Levels: A B C D E

