Readme
========================================================

1. The dataset
========================================================

The data analyzed here is the Human Activity Recognition Using Smartphones Data Set, a database built from the recordings of 30 subjects performing activities of daily living (ADL) while carrying a waist-mounted smartphone with embedded inertial sensors. 

It was donated to the Center for Machine Learning and Intelligent Systems at the University of California Irvine in 2012. Download and full information about the dataset are available on the UCI Machine Learning Repository website:

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

The data consists in:

**a. Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.**

The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. 
The body acceleration signal obtained by subtracting the gravity from the total acceleration. 

The sets for total acceleration are identified as:

```total_ acc_ {x,y,z}_{train,test}```

The sets for body acceleration are identified as:

```body_ acc_ {x,y,z}_{train,test}```


**b. Triaxial Angular velocity from the gyroscope.**

The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

The sets are identified as:

```body_ gyro_ {x,y,z}_{train,test}```

**c. A 561-feature vector with time and frequency domain variables.** 

The sets are identified as: 

``` X_{train,test} ```


**d. Its activity label.**

The sets are identified as: 

```y_{train, test}```


**e. An identifier of the subject who carried out the experiment.**

Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 

The sets are identified as:

```subject_{train, test}```


- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.

2. The transforms used to merge the data
========================================================

Obtained the sets, we first appended the test sets to the train sets in each of them using the form 

```variable <- rbind(variable_ train, variable_ test)```

To make life easier, we obtained the names of the variables in ```X``` in the "features" document and applied them to the ```X``` data frame. 

Then we first labeled, then appended the tables ```y``` (activity) and ```subject``` (identification) to the beginning of the ```X``` data frame. Activities were described with their proper names. 

We named this big data frame ```total```, with 563 columns. The first two are labels (subject and activity), the other 561 are measures.

Since variables in the data sets from the "Inertial Signals" folder were not named, and aiming to avoid confusion in a very technical field I don't really understand, those triaxial data frames were not merged. So, let's do with the properly labeled columns. 

Variable names were "translated" into something more properly readable. From the hints in the ```features_info.txt``` file, copied the variable names from the ```features``` data frame into a vector and replaced them with more readable names. 

For example:

```tBodyAcc-mean()-X``` became ```TimeBodyAccelerationMeanXAxis```

```tGravityAcc-arCoeff()-Z,1``` became ```TimeGravityAccelerationAutoRegressionCoefficient()ZAxis_1```

Yes, they are long. But they are much more readable this way, even if I don't really know what a Fourier transform actually does. 

For that, we used a long chain of instances of the ```grep``` function. It seemed flexible enough, even if repetitive, to deal with all the nuances in the data set. 


3. Tidying it up
========================================================

The goal of the project was to create a "tidy" dataset with the mean and standard deviation of each activity in each variable for each individual. 

This is a short definition of what can be considered tidy data:

- Each column a variable;
- Each line an observation;
- Each table a kind of data

This is what we had:

- 561 original variables
- 30 individuals
- 6 activities
- 2 measures, mean and standard deviation

For the purpose of the task, and after reading remarks from TAs about how they did it, we simplified the final data load by choosing only two variables to do it: ```TimeBodyAccelerationSignalMagnitudenitudeArea()``` and ```TimeGravityAccelerationSignalMagnitudenitudeArea()```. That reduces the scope. 

Then we melted the data, by subject and activity. The melted data was split by variable. 

From each variable dataset, using dcast, we took the mean and then standard deviation for each variable by subject and activity, resulting in a 7-column table: 

- subject
- mean/sd of "LAYING"
- mean/sd of "SITTING"
- mean/sd of "STANDING"
- mean/sd of "WALKING"
- mean/sd of "WALKING_ DOWNSTAIRS"
- mean/sd of "WALKING_UPSTAIRS"

To that, we added one column for each data frame describing the kind of measure (mean for mean, standard deviation for sd) and the name of the variable. We used ```rbind``` to put it all together. 

In the final data frame, we have now nine columns:

- subject
- mean/sd of "LAYING"
- mean/sd of "SITTING"
- mean/sd of "STANDING"
- mean/sd of "WALKING"
- mean/sd of "WALKING_ DOWNSTAIRS"
- mean/sd of "WALKING_UPSTAIRS"
- measure (mean or standard deviation)
- variable

I'd rather include measure and variable as second and third columns, but it would take some extra time. 

This is what you have on the ```result``` data frame, with 120 observations of nine variables. 

The data frame was exported as a .txt file, for the purposes of the peer evaluation.