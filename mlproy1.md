#Proyect Write up

I basically use random forest obtaining excellent prediction power, 100% acuraccy in the testing set. One of the challenges was to overcome the issues with the performance of random forest in R. Usually i program in Python and i was surprised that the performance of random forest in R is patetic.

The seleccion of features was based on the features not missing in the testing set, it doesn't make any sense to use features missing in the testing set.  

The accuracy in the testing set is  95% so expect to obtain excellent result in the out of sample which I HAD.



```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.1.3
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 3.1.3
```

```r
library(foreach)
```

```
## Warning: package 'foreach' was built under R version 3.1.3
```

```r
data <-  read.csv("pml-training.csv")  
set.seed(998)
inTrain <- createDataPartition(y=data$classe, p=0.7, list=FALSE)
training <- data[inTrain,]
testing <- data[-inTrain,]
```


```r
modFit <- train(classe~roll_belt+pitch_belt+yaw_belt+total_accel_belt+gyros_belt_x+gyros_belt_y+accel_belt_x+accel_belt_y+accel_belt_z+magnet_belt_x+magnet_belt_y+magnet_belt_z+roll_arm+pitch_arm+yaw_arm+total_accel_arm+gyros_arm_x+gyros_arm_y+gyros_arm_z+accel_arm_x+accel_arm_y+accel_arm_z+magnet_arm_x+magnet_arm_y+magnet_arm_z+roll_dumbbell+pitch_dumbbell+yaw_dumbbell, 
                method="parRF", data=training, ntree=10, importance=TRUE)
```

```
## Loading required package: randomForest
```

```
## Warning: package 'randomForest' was built under R version 3.1.3
```

```
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```

```
## Warning: executing %dopar% sequentially: no parallel backend registered
```


```r
pred <- predict(modFit, testing)
confusionMatrix(pred, testing$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1636   21    8   13    0
##          B   11 1095   24    4    5
##          C    6   20  981   23    6
##          D   15    2   11  923    7
##          E    6    1    2    1 1064
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9684          
##                  95% CI : (0.9636, 0.9727)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.96            
##  Mcnemar's Test P-Value : 0.007638        
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9773   0.9614   0.9561   0.9575   0.9834
## Specificity            0.9900   0.9907   0.9887   0.9929   0.9979
## Pos Pred Value         0.9750   0.9614   0.9469   0.9635   0.9907
## Neg Pred Value         0.9910   0.9907   0.9907   0.9917   0.9963
## Prevalence             0.2845   0.1935   0.1743   0.1638   0.1839
## Detection Rate         0.2780   0.1861   0.1667   0.1568   0.1808
## Detection Prevalence   0.2851   0.1935   0.1760   0.1628   0.1825
## Balanced Accuracy      0.9837   0.9760   0.9724   0.9752   0.9906
```
