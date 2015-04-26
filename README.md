# PML-Class-Project
Repo containing Writeup for Practical Machine Learning Course

All analyses were performed using R version 3.2.0 (2015-04-16) -- "Full of Ingredients" Copyright (C) 2015 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

Initial exploratory analysis of "pml.training" dataset revealed that of the 160 variables presented in  the data structure, only 52 variables represented actual raw data. The training dataset was distilled down to the these 52 variables along with the outcome "classe" variable and renamed "cleantrain":

> colnames(cleantrain)
 [1] "roll_belt"            "pitch_belt"          
 [3] "yaw_belt"             "total_accel_belt"    
 [5] "gyros_belt_x"         "gyros_belt_y"        
 [7] "gyros_belt_z"         "accel_belt_x"        
 [9] "accel_belt_y"         "accel_belt_z"        
[11] "magnet_belt_x"        "magnet_belt_y"       
[13] "magnet_belt_z"        "roll_arm"            
[15] "pitch_arm"            "yaw_arm"             
[17] "total_accel_arm"      "gyros_arm_x"         
[19] "gyros_arm_y"          "gyros_arm_z"         
[21] "accel_arm_x"          "accel_arm_y"         
[23] "accel_arm_z"          "magnet_arm_x"        
[25] "magnet_arm_y"         "magnet_arm_z"        
[27] "roll_dumbbell"        "pitch_dumbbell"      
[29] "yaw_dumbbell"         "total_accel_dumbbell"
[31] "gyros_dumbbell_x"     "gyros_dumbbell_y"    
[33] "gyros_dumbbell_z"     "accel_dumbbell_x"    
[35] "accel_dumbbell_y"     "accel_dumbbell_z"    
[37] "magnet_dumbbell_x"    "magnet_dumbbell_y"   
[39] "magnet_dumbbell_z"    "roll_forearm"        
[41] "pitch_forearm"        "yaw_forearm"         
[43] "total_accel_forearm"  "gyros_forearm_x"     
[45] "gyros_forearm_y"      "gyros_forearm_z"     
[47] "accel_forearm_x"      "accel_forearm_y"     
[49] "accel_forearm_z"      "magnet_forearm_x"    
[51] "magnet_forearm_y"     "magnet_forearm_z"    
[53] "classe" 

"pml.testing" also had the same variable's removed to match "cleantrain" without the "classe" variable and renamed "cleantest".

Next, "cleantrain" was split into a training and test set using creatDataPartition():

> inTrain<- createDataPartition(y = cleantrain$classe, p = 0.80, list = FALSE)
> training <- cleantrain[inTrian,]
> testing <- cleantrain[-inTrain,]

As each model was trained, a new "training" and "testing" dataset was sampled from the "cleantrain" dataset by changing the seed, thereby prevent overfitting to the "testing" dataset.

With "training" and "testing" datasets established, models using the machine learning methods of random forests, linear discriminant analysis, bagged trees, and naive bayes were trained on the "training" dataset and cross-validated by fitting the models to the "training" dataset. Of these models, the random forest method proved to have the lowest out of sample error estimate as measured by 1 - Accuracy recorded on the "testing" set (.46%).

From here the random forest model was used to predict on the twenty test cases in "clean test" and submitted for grading.

Analysis Data

Random Forest
> set.seed(1)
> inTrain<- createDataPartition(y = cleantrain$classe, p = 0.80, list = FALSE)
> training <- cleantrain[inTrian,]
> testing <- cleantrain[-inTrain,]
> modelFit<- randomForest(classe ~., data=training)
> predicition<- predict(modelFit, newdata = testing)
> confusionMatrix(prediction, testing$classe)

Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1115    2    0    0    0
         B    1  756    5    0    0
         C    0    1  678    6    0
         D    0    0    1  636    1
         E    0    0    0    1  720

Overall Statistics
                                          
               Accuracy : 0.9954          
                 95% CI : (0.9928, 0.9973)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.9942          
 Mcnemar's Test P-Value : NA              

Linear Discriminant Analysis
> set.seed(2) 
> inTrain<- createDataPartition(y = cleantrain$classe, p = 0.80, list = FALSE)
> training <- cleantrain[inTrian,]
> testing <- cleantrain[-inTrain,]
> modelFit<- train(classe ~., data = training, method = "lda")
> predicition<- predict(modelFit, newdata = testing)
> confusionMatrix(prediction, testing$classe)

Confusion Matrix and Statistics

          Reference
Prediction   A   B   C   D   E
         A 914 116  46  39  26
         B  23 485  70  24 117
         C  90  97 470  82  83
         D  85  26  80 478  56
         E   4  35  18  20 439

Overall Statistics
                                          
               Accuracy : 0.7102          
                 95% CI : (0.6957, 0.7243)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.6335          
 Mcnemar's Test P-Value : < 2.2e-16       


Bagged Tress
> set.seed(3)
> inTrain<- createDataPartition(y = cleantrain$classe, p = 0.80, list = FALSE)
> training <- cleantrain[inTrian,]
> testing <- cleantrain[-inTrain,]
> modelFit<- train(classe ~., data = training, method = "treebag")
> predicition<- predict(modelFit, newdata = testing)
> confusionMatrix(prediction, testing$classe)

Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1108    9    1    2    0
         B    3  740    0    0    2
         C    3    8  676    7    1
         D    1    2    7  632    0
         E    1    0    0    2  718

Overall Statistics
                                          
               Accuracy : 0.9875          
                 95% CI : (0.9835, 0.9907)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2e-16         
                                          
                  Kappa : 0.9842          
 Mcnemar's Test P-Value : 0.02625         


Naive Bayes
> set.seed(4)
> inTrain<- createDataPartition(y = cleantrain$classe, p = 0.80, list = FALSE)
> training <- cleantrain[inTrian,]
> testing <- cleantrain[-inTrain,]
> modelFitn<- train(classe ~., data = training, method = "nb")
> predicition<- predict(modelFit, newdata = testing)
> confusionMatrix(prediction, testing$classe)

Confusion Matrix and Statistics

          Reference
Prediction   A   B   C   D   E
         A 984 182 175 138  36
         B  16 459  38   4  57
         C  34  48 439  67  25
         D  74  58  26 394  25
         E   8  12   6  40 578

Overall Statistics
                                          
               Accuracy : 0.7275          
                 95% CI : (0.7133, 0.7414)
    No Information Rate : 0.2845          
    P-Value [Acc > NIR] : < 2.2e-16       
                                          
                  Kappa : 0.6508          
 Mcnemar's Test P-Value : < 2.2e-16       


Conclusion:

   The random forest method outperformed all other methods investigated for this analysis. The out of sample estimate for the random forest model was .46%. The random forest model was then used against "cleantest" dataset and the following predictions were obtained:
   
prediction
 1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
 B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
Levels: A B C D E 
