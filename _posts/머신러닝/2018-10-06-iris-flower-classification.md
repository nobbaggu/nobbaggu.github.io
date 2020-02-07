---
title: (머신러닝) Iris-flower Classification
date: 2018-10-06T10:43:37+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - classification
  - code
  - flower
  - iris
  - machine learning
---
<pre><span style="color: #0000ff;"><span style="color: #000000;">################### Iris Flower Classification ####################
</span>
from</span> pandas <span style="color: #0000ff;">import</span> read_csv
<span style="color: #0000ff;">from</span> pandas <span style="color: #0000ff;">import</span> set_option
<span style="color: #0000ff;">from</span> numpy <span style="color: #0000ff;">import</span> set_printoptions
<span style="color: #0000ff;">from</span> matplotlib <span style="color: #0000ff;">import</span> pyplot
<span style="color: #0000ff;">from</span> pandas.plotting <span style="color: #0000ff;">import</span> scatter_matrix
<span style="color: #0000ff;">from</span> sklearn.feature_selection <span style="color: #0000ff;">import</span> SelectKBest
<span style="color: #0000ff;">from</span> sklearn.feature_selection <span style="color: #0000ff;">import</span> chi2
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> train_test_split
<span style="color: #0000ff;">from</span> sklearn.linear_model <span style="color: #0000ff;">import</span> LogisticRegression as LR
<span style="color: #0000ff;">from</span> sklearn.discriminant_analysis <span style="color: #0000ff;">import</span> LinearDiscriminantAnalysis as LDA
<span style="color: #0000ff;">from</span> sklearn.neighbors <span style="color: #0000ff;">import</span> KNeighborsClassifier as KNC
<span style="color: #0000ff;">from</span> sklearn.pipeline <span style="color: #0000ff;">import</span> FeatureUnion
<span style="color: #0000ff;">from</span> sklearn.pipeline <span style="color: #0000ff;">import</span> Pipeline
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> KFold
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> cross_val_score
<span style="color: #0000ff;">from</span> sklearn.ensemble <span style="color: #0000ff;">import</span> RandomForestClassifier as RFC
<span style="color: #0000ff;">from</span> sklearn.ensemble <span style="color: #0000ff;">import</span> BaggingClassifier
<span style="color: #0000ff;">from</span> sklearn.externals.joblib <span style="color: #0000ff;">import</span> dump

<span style="color: #999999;">#### data load ####</span>
url = <span style="color: #14d917;">'https://goo.gl/mLmoIz'</span> <span style="color: #999999;">## Iris-flower classification dataset</span>
names = [<span style="color: #14d917;">'sep_len'</span>, <span style="color: #14d917;">'sep_wid'</span>, <span style="color: #14d917;">'pet_len'</span>, <span style="color: #14d917;">'pet_wid'</span>, <span style="color: #14d917;">'class'</span>]
data = read_csv(url, names = names)                
array = data.values
X = array[:,0:4]
Y = array[:,4]

<span style="color: #999999;">#### data statistics ####</span>
set_option(<span style="color: #14d917;">'display.width'</span>, 100)
set_option(<span style="color: #14d917;">'precision'</span>, 3)
print(data.describe())
print(data.skew())
print(data.corr(method=<span style="color: #14d917;">'pearson'</span>))


<span style="color: #999999;">#### visualization of data ####</span>
data.plot(kind=<span style="color: #14d917;">'density'</span>, subplots=True, layout=(2,3), sharex=False)
pyplot.show()
data.plot(kind=<span style="color: #14d917;">'box'</span>, subplots=True, layout=(2,3), sharex=False, sharey=False)
pyplot.show()
scatter_matrix(data)
pyplot.show()


<span style="color: #999999;">#### feature selection ####</span>
test = SelectKBest(score_func=chi2, k=2)
fit = test.fit(X,Y)
features = fit.transform(X)
set_printoptions(precision=3)
print(X[0:5,:])
print(features[0:5,:])
X = features <span style="color: #999999;">##pet_len, pet_width 선택</span>

<span style="color: #14d917;">"""""""""""""""""""""""""""""""""""""""""""""""""""""""""</span>
<span style="color: #14d917;">#### feature scaling ####</span>
<span style="color: #14d917;"># scaler = MinMaxScaler(feature_range=(0,1))</span>
<span style="color: #14d917;"># rescaledX = scaler.fit_transform(X)</span>
<span style="color: #14d917;"># set_printoptions(precision=3)</span>
<span style="color: #14d917;"># print(rescaledX)</span>
<span style="color: #14d917;">"""""""""""""""""""""""""""""""""""""""""""""""""""""""""</span>
<span style="color: #999999;">### feature scailing은 모델의 성능을 향상시키지 못했다.</span>

<span style="color: #999999;">#### data split to training & validation test set ####</span>
X_train, X_validation, Y_train, Y_validation = train_test_split(X, Y, test_size=0.2, random_state=7)

<span style="color: #14d917;">"""""""""""""""""""""""""""""""""""""""""""""""""""""""""</span>
<span style="color: #14d917;">########pipeline을 통한 feature selection & model selection###########</span>
<span style="color: #14d917;">features = []</span>
<span style="color: #14d917;">features.append(('KBest', SelectKBest(score_func=chi2, k=2))) #KBest 알고리즘과 LDA 알고리즘 비교</span>
<span style="color: #14d917;">features.append(('lda', LDA()))</span>
<span style="color: #14d917;">feature_union = FeatureUnion(features) #더 좋은 점수를 얻은 feature selection 알고리즘 선택</span>
<span style="color: #14d917;">models = [('LR', LR()), ('Kneighbors', KNC()), ('LDA', LDA())]#Linear Regression, K-Nearest Neighbors, LDA 알고리즘 비교</span>
<span style="color: #14d917;">kfold = KFold(n_splits=10, random_state=7) # 10개의 fold dataset</span>
<span style="color: #14d917;">for name, model in models:</span>
<span style="color: #14d917;">   estimators=[]</span>
<span style="color: #14d917;">   estimators.append(('feature_union', feature_union))</span>
<span style="color: #14d917;">   estimators.append((name, model))</span>
<span style="color: #14d917;">   model_set = Pipeline(estimators) #pipeline을 통한 가장 좋은 모델 선택</span>
<span style="color: #14d917;">   result = cross_val_score(model_set, X_train, Y_train, cv=kfold)</span>
<span style="color: #14d917;">   print('%s: %.3f%% (%.3f%%)' %(name, result.mean()*100, result.std()*100)) #accuracy score</span>
<span style="color: #14d917;">"""""""""""""""""""""""""""""""""""""""""""""""""""""""""</span>
model = LDA() <span style="color: #999999;">## LDA가 가장 좋은 모델</span>
<span style="color: #999999;">### 위 pipeline 코드의 주석을 풀고 실행시키면 LDA가 가장 좋은 점수를 받는 것을 볼 수 있다.</span>

<span style="color: #999999;">#### model evaluation ####</span>
kfold = KFold(n_splits=10, random_state=7) # 10개의 fold dataset
results = cross_val_score(model, X_train, Y_train, cv=kfold)
print(<span style="color: #14d917;">'LDA Accuracy: %.3f%% (%.3f%%)'</span> %(result.mean()*100, result.std()*100))
LDA_result = results


<span style="color: #999999;">#### improve model performance ####</span>
X = array[:,0:4]
Y = array[:,4]
X_train, X_validation, Y_train, Y_validation = train_test_split(X, Y, test_size=0.2, random_state=7)
<span style="color: #999999;">### 이 전 번의 LDA model의 feature selection에서 fit을 하여 feature가 2개뿐이므로 원래 데이터 다시 복구</span>


model = BaggingClassifier(base_estimator=LDA(), n_estimators=100, random_state=7)
results = cross_val_score(model, X_train, Y_train, cv=kfold)
Bagging_result = results

num_trees = 100
max_features = 3
model = RFC(n_estimators = num_trees, max_features = max_features)
results = cross_val_score(model, X_train, Y_train, cv=kfold)
RandomForest_result = results

print(<span style="color: #14d917;">'LDA Accuracy: %.3f'</span> %(LDA_result.mean()*100))
print(<span style="color: #14d917;">'LDA Bagging Accuracy: %.3f'</span> %(Bagging_result.mean()*100))
print(<span style="color: #14d917;">'RandomForest Accuracy: %.3f'</span> %(RandomForest_result.mean()*100))        

<span style="color: #999999;">#### Save model ####</span>
<span style="color: #999999;"># model.fit(X,Y)</span>
<span style="color: #999999;"># filename = 'Iris_classification_model.sav'</span>
<span style="color: #999999;"># dump(model, filename)</span></pre>

&nbsp;

<img class="aligncenter size-full wp-image-1187" src="/images/2018/10/no-name.jpg" alt="" width="395" height="250" srcset="/images/2018/10/no-name.jpg 395w, /images/2018/10/no-name-300x190.jpg 300w" sizes="(max-width: 395px) 100vw, 395px" /> 

위 그림은 Iris flower 꽃잎의 정보를 담고있는 각 feature의 density plot이다. sep len과 sep wid는 normal distribution을 따른다는 것을 알 수 있다. pet len과 pet wid는 feature의 local optimum이 3개로 나뉜다. 우리 data의 class가 3가지의 꽃의 종류이므로 아무래도 pet len과 pet wid 두 feature가 분류를 하는 데 결정적인 역할을 하는 feature로 보인다. 실제로 KBest feature selection 알고리즘의 결과도 pet len과 pet feature를 뽑는다.

<img class="aligncenter size-full wp-image-1188" src="/images/2018/10/no-name-1.jpg" alt="" width="244" height="55" /> 

결과는 위와 같다. 가장 처음으로 구한 LDA model을 사용한 Ensemble(Bagging) 기법과 Random Forest Ensemble 기법 보다도 가장 먼저 구했던 LDA model의 분류 정확도가 가장 높다.