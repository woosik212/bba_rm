# bba_rm

# FINAL-MEU5053.01-00
Final Project - Machine Learning and Programming (MEU5053.01-00)


# Project Title
Detecting the changes of Land cover based on machine learning algorithms using Landsat Satellite Images

# Research Motivation and Background
Rapid urbanization across many regions in the world is altering the existing land use/land cover (LULC), which is significantly affecting the environment in cities; such as land surface temperature, air pollution, ecological environment, etc. Data related to land covers are built manually based on fact-findings surveys and manmade map, so the period of data production is long. This study aims to classify the land cover using machine learning algorithms and estimate the change of LULC in one of the fastest-growing megacities and the capital of Korea, named as Seoul.

## Study Area
![image](https://user-images.githubusercontent.com/68691092/121843798-e682a780-cd1d-11eb-8325-ac20133055a5.png)

# Data and Tools for Analysis
* MODIS – Landsat 8 OLI/TRS (resolution of 30m), 30m pixels with (rows: 1698, columns: 1500)
* 2013 Satellite Image – (2013. 09. 16) / 2020 Satellite Image – (2020. 04. 28) Cloud cover: Less than 1%
* Land cover of Korea (Environmental Geographical Information Service, 환경공간정보서비스, resolution of 30m)
* QGIS (GIS program for assigning and generating the training & testing sample), GRASS, GDAL
* Machine learning algorithms on scikit-learn

![image](https://user-images.githubusercontent.com/68691092/121843928-1f228100-cd1e-11eb-939c-f9aa7d4ac5d6.png)


## Get Satellite Imagery from the links below (Google Drive)
1) https://drive.google.com/file/d/1j-tCjUY2QR2MmBmpvHfVjYHtB7RXAmcM/view?usp=sharing
2) https://drive.google.com/file/d/1vimOAa93f7AkMtlgRGJptWk6U5OIdgCA/view?usp=sharing

There are two Landsat Images which are,
1) L8_Seoul_20130916.tif (Raster file, Landsat 8 image of Seoul on 16th Sep, 2013)
2) L8_Seoul_20200428.tif (Raster file, Landsat 8 image of Seoul on 28th April, 2020)

and we are going to use thess two raster files to calssifying the Land cover of Seoul and outskirts of Seoul
* The size of file is pretty big
* Be sure to assign the right path where you download two satellite image raster files (.tif).

## Get Train and Test Dataset (Github, Code -> Zip download).

# Methods
## Workflow Methodologies
In this study, the Random Forest, SVM(Support Vector Machine) methods were implemented to classify 4 classes of land cover (built-up, farm & barren, vegetation, and water) from 30m. resolution of Landsat 8 satellite image.
The existing landcover classification provided by NGIS was reclassified into 4 classes, and training and testing samples were randomly generated.
Among the machine learning techniques, the Random Forest and SVM (functions of linear, RBF, poly) algorithms were applied to train and test four classes of land covers from satellite images in year of 2013 and 2020.

![image](https://user-images.githubusercontent.com/68691092/121844055-4e38f280-cd1e-11eb-9381-03769ffff8a4.png)

## Distribution of Training & Testing samples
Total number of target samples are 886 points (built-up: 265, farm & barren: 157, vegetation: 283, water: 181) and training samples for 10 times the target samples.
Total number of testing samples are 249 points (built-up: 55, farm & barren: 52, vegetation: 65, water: 77).

![image](https://user-images.githubusercontent.com/68691092/121844128-732d6580-cd1e-11eb-827c-e263d39c7eb2.png)

# Results
## Random Forest
Random Forest also known as Random Decision Forest is an ensemble learning method that operates by constructing a several decision tress to generate the final mode of the classes.
This study train the classifier for both 2013 and 2020 satellite images to generate the landcover map.
Overall Accuracy/Kapp Statistics for each year of satellite image classification shows (2013) 0.887 / 0.848 and (2020) 0.876 / 0.833.
* [y2013, (n_estimator=100, max_depth=12, min_sample_leaf=3, min_sample_split=2)]
* [y2020, (n_estimators=100, max_depth=10, min_samples_leaf=3, min_samples_split=2)]

![image](https://user-images.githubusercontent.com/68691092/121844323-c7384a00-cd1e-11eb-8c1b-a4e4e63ed855.png)

One advantage of using RF is the insight into variable importance. In this case, the values in the 4th band contributed larger than any other bands for the model to detect classes.

![image](https://user-images.githubusercontent.com/68691092/121844348-d3240c00-cd1e-11eb-9ce3-bcf8f85668f0.png)

## Support Vector Machine
This study train the classifier for both 2013 and 2020 satellite images to generate the landcover map.
Overall Accuracy/Kapp Statistics for each year of satellite image classification shows:
 Function - “linear” - (2013) 0.871 / 0.826 and (2020) 0.815 / 0.751.
 Function - “rbf” - (2013) 0.896 / 0.859 and (2020) 0.855 / 0.805.
 Function - “poly” - (2013) 0.884 / 0.843 and (2020) 0.883 / 0.843.
 
 ### Tuning the hyper-parameters of an estimator
 * [y2013: SVC(C=1000, gamma=‘scale’, kernel =‘linear’)] / [y2020: SVC(C=10, gamma=‘scale’, kernel=‘linear’)]
 * [SVC(C=10, gamma=‘scale’, kernel =‘rbf’)]
 * [SVC(C=1000, gamma=‘scale’, kernel =‘ploy’)]

Grid search was used as an approach to hpyer-parameter tuning which would methodically build and evaluate a model for each combination of algorithm parameters specified in grid. For selecting the best hyper-parameter GridsearchCV from Scikit Learn was used. In python script, the code was buit based on a dictionary called param_grid and fill out some parameters of kernels, C and gamma. Then, built a GridSearchCV object and fit it to the training data. To get the optimal parameters, "grid.best_estimator_" was used, and the result was used to get the overall accuracy of each model "linear", "rbf", and "poly".

![image](https://user-images.githubusercontent.com/68691092/121844615-34e47600-cd1f-11eb-9978-391a2d7cac26.png)

## Accuracy Assessment - validation
RF shows the highest classification accuracy for satellite imagery in 2013, and SVM (“poly”) shows the highest classification accuracy for satellite imager in 2020.
Although classification for the 2020 image performed less, most likely, due to the noise from different soil and urban tree canopy.

![image](https://user-images.githubusercontent.com/68691092/121844923-a9b7b000-cd1f-11eb-91b0-c74ad8715dbf.png)

## Land cover change detection
Detecting the landcover changes in 7 years in between the raw images for 2013 and 2020 which are classified into 4 major classes.
Algorithm explanation: Conducting a pixel-to-pixel comparison, for those pixels that went through no change a value of 0 will be assigned, and for those pixels that went through changed to Built-up area will be re-coded as 1. Similarly, pixels in each class group will be assigned a newer value if change is detected. 

Accordingly, in the years between 2013 and 2020, the metropolitan area went through a significant change in built-up surface area.
The significant change to Vegetation cover, Farm & Barren, and Water most likely is the result of mis-classified pixels, particularly on the 2013 image.
The figures on the right shows the newly created Built-up surface in 2020 based on RF and SVM(“poly”).

![image](https://user-images.githubusercontent.com/68691092/121845420-5e51d180-cd20-11eb-895d-ec5a34b058f3.png)

# Discussion
## Random Forest and Support Vector Machine classifiers 

A landcover classification study using Landsat-8 and involving six land-cover classes found that SVM was able to achieve a relatively high overall accuracy of 88% (Goodin et al., 2015). The results of this study agrees with previous research which concluded that the selection of an appropriate features with the SVM classifier for mapping landcover (Waske et al. ,2010). Also, Ma, Fu et al. (2017) alleged that appropriate feature selection could significantly increase accuracy, especially for smaller sample sizes for the SVM and RF classifiers. The result of this study show similar accuracy to those of prior studies and show that land cover classification using SVM and RF algorithms can be somewhat accurate.

The kernel functions of SVM are used to map the original dataset (linear or non-linear) into a higher dimesional space with view to making it linear dataset. In pattern recognition like calssification of satellite images, one drawback of balanced training sampels in traditional accuracy assessment is that the proportions of land-cover classes are not taken into condsideration. The overall accuracy of this study may have biased due to ignore the area proportions of classes. However, this focus on overall accuracy take into account the performance of individual classes and is particularly advatageous to those that are under-represented. In this study, SVM polynominal kernel function produced the both highest accuracy and stable especially for Farm & Barren (open land) among 4 classes. The ability of SVM to capture Farm & Barren(open land and clear cuts) class where the data were linearly non-separable, the feature vectors were projected with a nonlinear vector mapping function to a higher dimension feature space (Abdi, 2020).

The selection of hyper parameter for SVM is done via grid search. This study set the C as the penalty hyper parameter of the error term in the training process. With a high C value, the algorithm will try to reduce the error in the training phase and, to achieve this, a complex surface as known as hyper plane will be formed to seperate the classes. If this surface is too complex with very high C, it will be shaped to cover each pattern of the training data, even noise. In this case, the classifier might have very good performance on the training data and lose in terms of generalization, resulting in a poor performance on the test set and overfitting problem occurred. On the other hand, if the hyperplan is too simple with very low C value, it may not recognize the important patterns of the data, leading high train and test errors and underfitting problem occured. In this study the grid search provides the best estimator (or best hpyer parameter) for each kernel functions of SVM. The result of gird search provides the best "C" values for each kernel function. Both the poly function showing the best estimator were estimated to have a C value of 1000, but the rbf function showing the C value of 10 and the linear function showing the different C values of 10 and 1000 for each satellite images. If the C value is too small, there is a possibility of underfit, and if the C value is too large, there is a possibility of overfit. The exact criteria for the overfit or underfit can be determined by cross validation, but in this study, no other validation existis because training data and test data are generated separately, they did not generate/split from the whole dataset. Theoretically, Random Forest algorithms show that overfitting does not occur even when the number of trees increases by a hundred. In this study, it  cannot be explained that the rbf function of the SVM has become an underfit, but we can confirm that the polyfunction of the SVM, which shows the most similar accuracy to RF, does not become an overfit and has a high accuracy. Although this study did not accurately diagnose the overfit problem, we identify the overfit problem of SVM by tuning the hyperparameter in the RF algorithm and showing similar accuracy as poly in SV

## Limitation of this study
* All 4 Land Cover classes didn’t have the same number of training and evaluation samples so that the Overall Accuracy could be biased towards classes with more training samples such as Built-up and Vegetation (He and Garcia, 2018).
* Limitations of data acquisition in satellite data: seasonal band spectral differences such as vegetation and farmland because different months are compared in this study. 
* The hyper-parameter optimization applied on this study was not based on iterative tuning and a random grid search, thus the accuracies reported on this study may not represent the maximum possible that could be attained.

# Conclusion
## Classifier comparison
This study compares the accuracy of the model through RF and SVM classifier’s algorithms, separating the 2013 and 2020 Landsat 8 – OLI/TIRS images into differ land cover classes. Overall accuracies of RF and SVM show similar to previous studies, in which the accuracy of models using “poly” function in SVM is the most stable over year of 2013 and 2020. Comparison of changes in the Built-up area in 2013 and 2020 shows that changes in the Farm & Barren and Water region have changed more in the SVM model than in RF.
These results in classifying land cover based on RGB/Spectral Bands values based on satellite imagery, the problem occurred due to the noise from different soil and tree canopy.

# Reference
* Adbi, (2020). Land cover and land use classification performance of machine learning algorithms in a boreal landscape using Sentinel-2 data. GIScinece & Remote Sensing, 57:1, 1-20
* Goodin et al., (2015). Mapping land cover and land use from Objected-based Classification: An example from a complex agricultural landscape. International Journal of Remote Sensing, 36 (18): 4702-4723
* Waske et al., (2010). Sensitivity of Support Vector Machines to Random Feature Selection in Classification Hyperspectral Data. IEEE Transactions on Geoscience and Remote Sensing, 48 (7): 2880-889
* Ma et al., (2017). Evaluation of Feature Selection Methods For Objected-based Land cover mapping of Unmanned Aerial Vehicle Imagery using random forest and support vector machine classifiers. International Journal of Geo-information, 6 (2): 51
* Georganos et al., (2018). Less is more: optimizing classification performance through feature selection in a very-high-resolution sensing object-based urban application, GISscience & Remote Sensing, 55:2, 221-242
* https://mljar.com/blog/random-forest-overfitting/
* https://github.com/woosik212/satimg

# Thank You
