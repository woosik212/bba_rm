# baaa_read me

# Bigdata analytics and applications (GSI7656.01-00)
Final Project


# Project Title
Landsat 위성영상을 이용한 머신러닝 알고리즘 기반 토지표지 변화 감지

# Research Motivation and Background
세계의 많은 지역에 걸친 급속한 도시화는 토지 표면 온도, 대기 오염, 생태 환경과 같은 도시의 환경에 상당한 영향을 미치는 기존의 토지 사용/토지 커버(LULC)를 변화시키고 있다. 토지 커버 관련 데이터는 실태조사 및 인공지도를 기반으로 수작업으로 구축되기 때문에 데이터 제작 기간이 길다. 이 연구는 머신러닝 알고리즘을 이용해 토지 면적을 분류하고, 서울이라는 한국의 가장 빠르게 성장하고 있는 대도시 중 하나이자 수도의 LULC 변화를 추정하는 것을 목적으로 한다.

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

두 개의 래스터 파일을 사용하여 서울과 서울 외곽의 토지 커버에 적용할 것이다.
* 파일의 크기가 꽤 크기 떄문에 다운로드 받을시 주의하길 바람
* 두 개의 위성 이미지 래스터 파일(.tif)을 다운로드하는 올바른 경로를 할당해야함

## Get Train and Test Dataset
# Methods
## Workflow Methodologies
본 연구에서는 Landsat 8 위성 이미지의 30m 해상도에서 4개 등급의 토지 커버(건설, 농장 및 불모, 식물, 물)를 분류하는 랜덤 포레스트, SVM(서포트 벡터 머신) 방법을 구현했다.
NGIS가 제공한 기존 육상 커버 분류는 4개 등급으로 재분류되었고, 훈련 및 테스트 샘플이 무작위로 생성되었다.
기계 학습 기법 중 랜덤 포레스트와 SVM(선형, RBF, 폴리 기능) 알고리즘을 적용하여 2013년과 2020년에 위성 이미지에서 4가지 등급의 토지 커버 훈련 및 테스트를 실시했다.

![image](https://user-images.githubusercontent.com/68691092/121844055-4e38f280-cd1e-11eb-9381-03769ffff8a4.png)

## Distribution of Training & Testing samples
총 표본 수는 886 points (built-up: 265, farm & barren: 157, vegetation: 283, water: 181) 대상 표본의 10배에 해당하는 훈련 표본을 가진다.
검사용 표본 수는 249 points (built-up: 55, farm & barren: 52, vegetation: 65, water: 77).

![image](https://user-images.githubusercontent.com/68691092/121844128-732d6580-cd1e-11eb-827c-e263d39c7eb2.png)

# Results
## Random Forest
본 연구는 2013년과 2020년 위성 이미지 모두에 대한 분류기를 훈련시켜 육상 커버 지도를 생성한다.
위성 이미지 분류의 각 연도별 전체 정확도/Kappa 통계는 다음과 같음 (2013) 0.887 / 0.848 and (2020) 0.876 / 0.833.
* [y2013, (n_estimator=100, max_depth=12, min_sample_leaf=3, min_sample_split=2)]
* [y2020, (n_estimators=100, max_depth=10, min_samples_leaf=3, min_samples_split=2)]

![image](https://user-images.githubusercontent.com/68691092/121844323-c7384a00-cd1e-11eb-8c1b-a4e4e63ed855.png)

RF를 사용하는 한 가지 장점은 가변적 중요성에 대한 통찰력(Insight)입니다. 이 경우 네 번째 대역(band)의 값은 모형이 클래스를 탐지하는 데 다른 대역보다 큰 기여를 하고 있다.

![image](https://user-images.githubusercontent.com/68691092/121844348-d3240c00-cd1e-11eb-9ce3-bcf8f85668f0.png)

## Support Vector Machine
본 연구는 2013년과 2020년 위성 이미지 모두에 대한 분류기를 훈련시켜 토지 피복 지도를 생성한다

Overall Accuracy/Kapp Statistics for each year of satellite image classification shows:
 Function - “linear” - (2013) 0.871 / 0.826 and (2020) 0.815 / 0.751.
 Function - “rbf” - (2013) 0.896 / 0.859 and (2020) 0.855 / 0.805.
 Function - “poly” - (2013) 0.884 / 0.843 and (2020) 0.883 / 0.843.
 
 ### Tuning the hyper-parameters of an estimator
 * [y2013: SVC(C=1000, gamma=‘scale’, kernel =‘linear’)] / [y2020: SVC(C=10, gamma=‘scale’, kernel=‘linear’)]
 * [SVC(C=10, gamma=‘scale’, kernel =‘rbf’)]
 * [SVC(C=1000, gamma=‘scale’, kernel =‘ploy’)]

Grid search는 그리드에 지정된 알고리즘 매개 변수의 각 조합에 대해 모델을 체계적으로 구축하고 평가하는 hpyer-parameter 튜닝에 대한 접근법으로 사용되었다.
Scikit Learn에서 최상의 하이퍼 파라미터 GridsearchCV를 선택하기 위해 사용되었습니다.
파이썬 스크립트에서 코드는 param_grid라고 불리는 사전을 기반으로 하며 커널, C, 감마 등의 매개 변수를 채웠다.
그런 다음 GridSearchCV 개체를 작성하여 교육 데이터에 맞춥니다. 최적의 매개 변수를 얻기 위해 "grid.best_estimator_"를 사용하였고, 결과는 각 모델 "선형", "rbf", "폴리"의 전체적인 정확도를 얻기 위해 사용되었다.

![image](https://user-images.githubusercontent.com/68691092/121844615-34e47600-cd1f-11eb-9978-391a2d7cac26.png)

## Accuracy Assessment - validation
RF는 2013년 위성 이미지에 대한 분류 정확도가 가장 높고, SVM("poly")은 2020년 위성 이미저의 분류 정확도가 가장 높은 것으로 나타났다.
2020년 이미지에 대한 분류는 다른 토양과 도시 나무 캐노피에서 발생하는 noise로 인해 수행 능력이 저하된 것으로 해석된다.

![image](https://user-images.githubusercontent.com/68691092/121844923-a9b7b000-cd1f-11eb-91b0-c74ad8715dbf.png)

## Land cover change detection
2013년과 2020년의 raw 이미지는 7년 동안의 토지 피복의 변화를 감지하며, 크게 4가지 등급으로 분류된다.
알고리즘에 대한 설명 > 픽셀 대 픽셀 비교를 수행하면서 변경되지 않은 픽셀에 대하여 0의 값이 할당되며, 빌드업 영역으로 변경된 픽셀에 대하여 1로 재 코딩된다.
마찬가지로, 변경 사항이 감지되면 각 클래스 그룹의 픽셀에 더 새로운 값이 할당된다.

이에 따라 2013년부터 2020년 사이 수도권 일대는 시가화지역 면적에 큰 변화를 보여주고 있다.
Vegetation, Far & Barren, Water의 중요한 변화는 2013년 이미지에서 픽셀이 잘못 분류된 결과일 가능성이 높다.
아래의 그림은 RF 및 SVM("poly")을 기반으로 2020년에 새롭게 생성된 시가화 지역이다.

![image](https://user-images.githubusercontent.com/68691092/121845420-5e51d180-cd20-11eb-895d-ec5a34b058f3.png)

# Conclusion
## Classifier comparison
본 연구는 RF 및 SVM 분류에 대한 알고리즘을 생성하고 모델의 정확도를 비교하여 2013년과 2020년 Landsat 8 -OLI/TIRS 이미지를 서로 다른 토지 커버 클래스로 분류하였다. RF와 SVM의 전반적인 정확도는 2013년과 2020년에 걸쳐 SVM의 "poly" 함수를 사용하는 모델의 정확도가 가장 안정적으로 도출되었다. 2013년과 2020년 시가화지역의 변화를 비교한 결과, RF보다 Farm & Barren, Water 지역의 변화가 SVM 모델에서 더 많이 변화했음을 알 수 있다. 이는 위성 이미지가 RGB 스팩트럼 밴드 값에 기반하여 토지 피복이 분류되는데, 서로 다른 토양과 나무의 canopy에서 발생하는 noise가 이러한 오류를 발생시켰을 수 있음을 시사하는 바이다.

# Reference
* Georganos et al., (2018). Less is more: optimizing classification performance through feature selection in a very-high-resolution sensing object-based urban application, GISscience & Remote Sensing, 55:2, 221-242
* https://mljar.com/blog/random-forest-overfitting/
* https://github.com/woosik212/satimg

# Thank You
