---
title: (머신러닝) Titanic Survival Prediction
date: 2018-10-07T14:03:36+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - kaggle
  - machine learning
  - model
  - prediction
  - survival
  - titanic
  - titanic survival
  - 캐글
---
머신러닝 competition 으로 유명한 플랫폼인 Kaggle에 입문할 때 가장 많이 다루는 titanic survival data로 구출자 예측에 관한 모델링을 해보았다. 이 dataset에는 분명 pattern이란 것이 나타난다. 구출된 사람의 데이터에는 남자보다는 여자, 청년보다는 노인과 어린아이, 더 높은 객석의 등급이라는 특징이 있다. 이 dataset으로 모델링을 하면 이후에도 승객의 데이터를 가지고 구출이 될 것인지 아닌지 예측을 할 수 있다.

<pre><span style="color: #0000ff;">
from</span> pandas <span style="color: #0000ff;">import</span> read_csv
<span style="color: #0000ff;">from</span> pandas <span style="color: #0000ff;">import</span> set_option
<span style="color: #0000ff;">from</span> numpy <span style="color: #0000ff;">import</span> set_printoptions
<span style="color: #0000ff;">from</span> matplotlib <span style="color: #0000ff;">import</span> pyplot
<span style="color: #0000ff;">from</span> pandas.plotting <span style="color: #0000ff;">import</span> scatter_matrix
<span style="color: #0000ff;">from</span> sklearn.feature_selection <span style="color: #0000ff;">import</span> SelectKBest
<span style="color: #0000ff;">from</span> sklearn.feature_selection <span style="color: #0000ff;">import</span> chi2
<span style="color: #0000ff;">from</span> sklearn.preprocessing <span style="color: #0000ff;">import</span> MinMaxScaler
<span style="color: #0000ff;">from</span> sklearn.preprocessing <span style="color: #0000ff;">import</span> StandardScaler
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> train_test_split
<span style="color: #0000ff;">from</span> sklearn.linear_model <span style="color: #0000ff;">import</span> LogisticRegression as LR
<span style="color: #0000ff;">from</span> sklearn.linear_model <span style="color: #0000ff;">import</span> Ridge
<span style="color: #0000ff;">from</span> sklearn.discriminant_analysis <span style="color: #0000ff;">import</span> LinearDiscriminantAnalysis as LDA
<span style="color: #0000ff;">from</span> sklearn.decomposition <span style="color: #0000ff;">import</span> PCA
<span style="color: #0000ff;">from</span> sklearn.neighbors <span style="color: #0000ff;">import</span> KNeighborsClassifier as KNC
<span style="color: #0000ff;">from</span> sklearn.pipeline <span style="color: #0000ff;">import</span> FeatureUnion
<span style="color: #0000ff;">from</span> sklearn.pipeline <span style="color: #0000ff;">import</span> Pipeline
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> KFold
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> cross_val_score
<span style="color: #0000ff;">from</span> sklearn.ensemble <span style="color: #0000ff;">import</span> RandomForestClassifier as RFC
<span style="color: #0000ff;">from</span> sklearn.ensemble <span style="color: #0000ff;">import</span> BaggingClassifier
<span style="color: #0000ff;">from</span> sklearn.externals.joblib <span style="color: #0000ff;">import</span> dump
<span style="color: #0000ff;">from</span> sklearn.externals.joblib <span style="color: #0000ff;">import</span> load
<span style="color: #0000ff;">import</span> pandas <span style="color: #0000ff;">as</span> pd
<span style="color: #0000ff;">import</span> numpy <span style="color: #0000ff;">as</span> np
<span style="color: #0000ff;">from</span> sklearn.model_selection <span style="color: #0000ff;">import</span> RandomizedSearchCV
<span style="color: #0000ff;">from</span> scipy.stats <span style="color: #0000ff;">import</span> uniform
<span style="color: #0000ff;">from</span> sklearn.metrics <span style="color: #0000ff;">import</span> accuracy_score

<span style="color: #999999;">#### data load ####</span>
filename = <span style="color: #3b9931;">'titanic_survival_train.csv'</span>
names = []
data = read_csv(filename)
data.pop(<span style="color: #3b9931;">'PassengerId'</span>)  <span style="color: #999999;"># delete 4 unrelated data</span>
data.pop(<span style="color: #3b9931;">'Name'</span>)
data.pop(<span style="color: #3b9931;">'Cabin'</span>)
data.pop(<span style="color: #3b9931;">'Ticket'</span>)

print(data.isnull().any())  <span style="color: #999999;"># NaN data verification</span>

sex_mapping = {<span style="color: #3b9931;">'male'</span>: <span style="color: #993300;">1</span>, <span style="color: #3b9931;">'female'</span>: <span style="color: #993300;">2</span>}  <span style="color: #999999;"># character data -&gt; numeric data</span>
data[<span style="color: #3b9931;">'Sex'</span>] = data[<span style="color: #3b9931;">'Sex'</span>].map(sex_mapping)

embark_mapping = {<span style="color: #3b9931;">'C'</span>: <span style="color: #993300;">1</span>, <span style="color: #3b9931;">'S'</span>: <span style="color: #993300;">2</span>, <span style="color: #3b9931;">'Q'</span>: <span style="color: #993300;">3</span>}  <span style="color: #999999;"># character data -&gt; numeric data</span>
data[<span style="color: #3b9931;">'Embarked'</span>] = data[<span style="color: #3b9931;">'Embarked'</span>].map(embark_mapping)

data.Age = data.Age.fillna(pd.DataFrame.median(data.Age))  <span style="color: #999999;"># NaN -&gt; median value</span>
data.Embarked = data.Embarked.fillna(<span style="color: #993300;">2</span>)  <span style="color: #999999;"># NaN -&gt; highest freuquent value</span>

array = data.values
Y = array[:, 0]
X = array[:, 1:]

<span style="color: #999999;">#### data statistics ####</span>
set_option(<span style="color: #3b9931;">'display.width'</span>, <span style="color: #993300;">100</span>)
set_option(<span style="color: #3b9931;">'precision'</span>, <span style="color: #993300;">2</span>)
<span style="color: #800080;">print</span>(data.describe())
<span style="color: #800080;">print</span>(data.skew())
<span style="color: #800080;">print</span>(data.corr(method=<span style="color: #3b9931;">'pearson'</span>))
<span style="color: #800080;">print</span>(data.groupby(<span style="color: #3b9931;">'Survived'</span>).size())

<span style="color: #999999;">#### visualization of data ####</span>
data.plot(kind=<span style="color: #3b9931;">'density'</span>, subplots=<span style="color: #800080;">True</span>, layout=(3, 3), sharex=<span style="color: #800080;">False</span>)
pyplot.show()
data.plot(kind=<span style="color: #3b9931;">'box'</span>, subplots=<span style="color: #800080;">True</span>, layout=(3, 3), sharex=<span style="color: #800080;">False</span>, sharey=<span style="color: #800080;">False</span>)
pyplot.show()
scatter_matrix(data)
pyplot.show()

<span style="color: #999999;">#### feature scaling ####</span>
scaler = MinMaxScaler(feature_range=(<span style="color: #993300;"></span>, <span style="color: #993300;">1</span>))
rescaledX = scaler.fit_transform(X)
set_printoptions(precision=3)
X = rescaledX
<span style="color: #800080;">print</span>(X)


<span style="color: #999999;">#### feature selection ####</span>
kbest = SelectKBest(score_func=chi2, k=<span style="color: #993300;">4</span>)
fit = kbest.fit(X, Y)
features = fit.transform(X)
set_printoptions(precision=3)
X = features
<span style="color: #800080;">print</span>(X)

X_tr, X_test, Y_tr, Y_test = train_test_split(X, Y, test_size=<span style="color: #993300;">0.2</span>, random_state=<span style="color: #993300;">7</span>)
X = X_tr
Y = Y_tr

<span style="color: #3b9931;">"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""</span>
<span style="color: #3b9931;">########pipeline을 통한 feature selection & model selection###########</span>
<span style="color: #3b9931;">features = []</span>
<span style="color: #3b9931;">features.append(('KBest', SelectKBest(score_func=chi2, k=3))) #KBest 알고리즘과 LDA 알고리즘 비교</span>
<span style="color: #3b9931;">features.append(('lda', LDA()))</span>
<span style="color: #3b9931;">feature_union = FeatureUnion(features) #더 좋은 점수를 얻은 feature selection 알고리즘 선택</span>
<span style="color: #3b9931;">models = [('LogisticRegression', LR()), ('K-Nearest Neighbors', KNC()), ('LDA', LDA())]#Linear Regression, K-Nearest Neighbors, LDA 알고리즘 비교</span>
<span style="color: #3b9931;">kfold = KFold(n_splits=10, random_state=7) # 10개의 fold dataset</span>
<span style="color: #3b9931;">for name, model in models:</span>
<span style="color: #3b9931;">   estimators=[]</span>
<span style="color: #3b9931;">   estimators.append(('feature_union', feature_union))</span>
<span style="color: #3b9931;">   estimators.append((name, model))</span>
<span style="color: #3b9931;">   model_set = Pipeline(estimators) #pipeline을 통한 가장 좋은 모델 선택</span>
<span style="color: #3b9931;">   result = cross_val_score(model_set, X, Y, cv=kfold)</span>
<span style="color: #3b9931;">   print('%s: %.3f%% (%.3f%%)' %(name, result.mean()*100, result.std()*100)) #accuracy score</span>
<span style="color: #3b9931;">"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""</span>

models = []
models.append((<span style="color: #3b9931;">'LR'</span>, LR()))
models.append((<span style="color: #3b9931;">'KNN'</span>, KNC()))
models.append((<span style="color: #3b9931;">'RF'</span>, RFC(n_estimators=<span style="color: #993366;">100</span>, max_features=<span style="color: #993366;">4</span>)))

kfold = KFold(n_splits=<span style="color: #993366;">10</span>, random_state=<span style="color: #993366;">7</span>)  <span style="color: #999999;"># 10개의 fold dataset</span>
<span style="color: #0000ff;">for</span> name, model <span style="color: #0000ff;">in</span> models:
    results = cross_val_score(model, X, Y, cv=kfold)
    print(<span style="color: #3b9931;">'%s cross validation Accuracy: %.3f%% (%.3f%%)'</span> % (name, results.mean() * <span style="color: #993366;">100</span>, results.std() * <span style="color: #993366;">100</span>))
<span style="color: #999999;">#### K-Nearest Neighbors Classifer model이 가장 좋은 점수</span>

</pre>

<pre><span style="color: #0000ff;">for</span> name, model <span style="color: #0000ff;">in</span> models:
    model.fit(X,Y)
    prediction = model.predict(X_test)
    print(<span style="color: #3b9931;">'%s test accuracy: %.3f%%'</span> %(name, accuracy_score(Y_test, prediction)*<span style="color: #993366;">100</span>))</pre>

<pre><span style="color: #999999;">#### Improve Model Performance(hyper-parameter tuning, ensemble method) ####</span>
<span style="color: #999999;"> # model = BaggingClassifier(base_estimator=LR(), n_estimators=100, random_state=7)</span>
<span style="color: #999999;"> # results = cross_val_score(model, X, Y, cv=kfold)</span>
<span style="color: #999999;"> # Bagging_result = results</span>
<span style="color: #999999;"> #</span>
<span style="color: #999999;"> # num_trees = 100</span>
<span style="color: #999999;"> # max_features = 3</span>
<span style="color: #999999;"> # model = RFC(n_estimators = num_trees, max_features = max_features)</span>
<span style="color: #999999;"> # results = cross_val_score(model, X, Y, cv=kfold)</span>
<span style="color: #999999;"> # RandomForest_result = results</span>
<span style="color: #999999;"> #</span>
<span style="color: #999999;"> # print('LR Accuracy: %.3f%%' %(LR_result.mean()*100))</span>
<span style="color: #999999;"> # print('LR Bagging Accuracy: %.3f%%' %(Bagging_result.mean()*100))</span>
<span style="color: #999999;"> # print('RandomForest Accuracy: %.3f%%' %(RandomForest_result.mean()*100)) </span> 


<span style="color: #999999;">#### model save ####</span>
models[<span style="color: #993366;">1</span>][<span style="color: #993366;">1</span>].fit(X, Y)
filename = <span style="color: #3b9931;">'titanic_survival_prediction_%s.sav'</span> % models[<span style="color: #993366;">1</span>][<span style="color: #993366;"></span>]
dump(models[<span style="color: #993366;">1</span>][<span style="color: #993366;">1</span>], filename)</pre>

&nbsp;

<img class="aligncenter size-full wp-image-1202" src="/images/2018/10/no-name-1.jpg" alt="" width="618" height="181" srcset="/images/2018/10/no-name-1.jpg 618w, /images/2018/10/no-name-1-300x88.jpg 300w" sizes="(max-width: 618px) 100vw, 618px" /> 

위는 feature의 correlation 정보이다. 절대값이 1이면 완벽한 correlation, 0으로 갈수록 서로 연관이 없단 소리다.

Survived와 깊게 관련된 feature는 Pclass, Sex, Fare 정도로 보인다. 남성보단 여성이, 그리고 더 좋은 등급의 Fare를 가진 계급의 사람이 통계적으로 더 많이 구출되었단 소리다.

그런데 Raw Data를 불러오면 Character로 된 feature가 있다. 예를들어 Sex 같은 경우는 남자는 &#8216;male&#8217;, 여자는 &#8216;female&#8217;이라는 문자열로 되어있는데 map 함수를 사용하여 숫자 데이터로 변환해야한다. 또한 NaN 으로 표시되는 정보가 비어있는 데이터가 있는데 이것은 각 feature의 median 값으로 채워넣었다. pandas 라이브러리의 fillna 함수를 사용하면 가능하다.

<img class="aligncenter size-full wp-image-1205" src="/images/2018/10/no-name-3.jpg" alt="" width="431" height="161" srcset="/images/2018/10/no-name-3.jpg 431w, /images/2018/10/no-name-3-300x112.jpg 300w" sizes="(max-width: 431px) 100vw, 431px" /> 

아무튼 Logistic Regression, K-Nearest Neighbors, Random Forest 3개의 모델을 사용하여 결과를 구하면 다음과 같다. cross validation 점수와 test data의 예측 결과가 조금 다르다. 단순히 data의 예측 불가능함의 우연에서 온 것인지 bias-variance trade-off에 의한 결과인지는 조금 더 봐야 알겠다.

이 문제를 이후로도 여러 모델을 사용하여 parameter tuning과 데이터 전처리 스킬을 늘린 후 accuracy 높이기에 도전해 볼 생각이다.

&nbsp;

&nbsp;