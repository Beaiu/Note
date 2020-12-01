## 教師あり学習

## 線形回帰 (scikit-learn)
https://qiita.com/0NE_shoT_/items/08376b08783cd554b02e 

**↓↓↓SAMPLE CODE↓↓↓**
```python
import sys
import os
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn import preprocessing
from sklearn.metrics import mean_absolute_error, mean_squared_error

from sklearn.datasets import load_boston
boston = load_boston()    # データセットの読み込み

import pandas as pd
env_data = pd.DataFrame(boston.data, columns = boston.feature_names)    # 説明変数(boston.data)をDataFrameに保存
price_data = boston.target    # 目的変数(boston.target)

## correlation (相関行列)
import seaborn as sns
corr_mat = env_data.corr(method="pearson")    # 各列の間の相関係数を算出
sns.heatmap(corr_mat, vmax=1, vmin=-1, center=0, annot=True, fmt=".1f")

## correlation2
X = env_data[['RM']]
Y = price_data
%matplotlib inline
lr = LinearRegression()
lr.fit(X, Y)
plt.scatter(X, Y, color = 'blue')         # 説明変数と目的変数のデータ点の散布図をプロット
plt.plot(X, lr.predict(X), color = 'red') # 回帰直線をプロット
plt.title('Regression Line')               # 図のタイトル
plt.xlabel('Average number of rooms [RM]') # x軸のラベル
plt.ylabel('Prices in $1000\'s [MEDV]')    # y軸のラベル
plt.grid()                                 # グリッド線を表示
plt.show()                                 # 図の表示

## split
X_train, X_test, y_train, y_test = train_test_split(env_data, price_data, test_size= 0.3, random_state=0)

## scaling
scaler = preprocessing.StandardScaler()
scaler.fit(X_train.values)
train_feature = scaler.transform(X_train.values)
test_feature = scaler.transform(X_test.values)

## training
LR = LinearRegression(normalize=False)
LR.fit(train_feature, y_train)
coef = LR.coef_
predict_Y_test = LR.predict(test_feature)

print("coef = ", coef)    # 説明変数の係数を出力
print("intercept = ", LR.intercept_)    # 切片を出力
#print("predict = ", LR.predict(test_feature))
#print("y_test = ", y_test)

## evaluate(線形回帰モデルの性能評価)
#残差プロット
plt.scatter(predicted_Y_test, predicted_Y_test - y_test, color = 'blue')      # 残差をプロット 
plt.hlines(y = 0, xmin = -10, xmax = 50, color = 'black') # x軸に沿った直線をプロット
plt.title('Residual Plot')                                # 図のタイトル
plt.xlabel('Predicted Values')                            # x軸のラベル
plt.ylabel('Residuals')                                   # y軸のラベル
plt.grid()                                                # グリッド線を表示
plt.show()                                               # 図の表示

#平均二乗誤差：残差平方和をデータ数で正規化した値(誤差が小さいほどモデルの性能は良い)
print( "RMSE(test)Root Mean Square : " + str(np.sqrt(mean_squared_error(y_test, predicted_Y_test))) + "    MAE(test)Mean Absolute Error: "+ str(mean_absolute_error(y_test, predicted_Y_test)))

#決定係数
print("score = ", LR.score(test_feature,y_test))
```

## ロジスティック回帰
http://ailaby.com/logistic_reg/#id3
https://qiita.com/0NE_shoT_/items/b702ab482466df6e5569
https://www.javatpoint.com/linear-regression-vs-logistic-regression-in-machine-learning

## スケール変換（データ前処理）
スケール変換は、扱う数値データを何らかの規則で変換するものである。機械学習で桁数の異なるデータをまとめて扱うときには、スケール変換がほぼ必須となる。  
https://helve-python.hatenablog.jp/entry/scikitlearn-scale-conversion
* StandardScaler: 標準化（平均0, 分散1）
* RobustScaler: 外れ値に頑健な標準化
* MinMaxScaler: 正規化（最大1, 最小0）


## cross-validation
https://blog.csdn.net/xiaodongxiexie/article/details/71915259?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf  
**↓↓↓SAMPLE CODE↓↓↓**
```python
from sklearn.datasets import load_boston  
from sklearn.linear_model import LinearRegression  
from sklearn.model_selection import KFold  
import pandas as pd  

boston = load_boston()  
env_data = pd.DataFrame(boston.data, columns = boston.feature_names)  
price_data = boston.target  

#線形回帰モデルの呼び出し 
lr = LinearRegression()     

# cross_val_score
# 对数据集进行指定次数的交叉验证并为每次验证效果评测
result = cross_val_score(lr, env_data, price_data, cv = 10)
print(result)

# KFold
# 验证数据取自训练数据，但不参与训练，这样可以相对客观的评估模型对于训练集之外数据的匹配程度。模型在验证数据中的评估常用的是交叉验证，又称循环验证。它将原始数据分成K组(K-Fold)，将每个子集数据分别做一次验证集，其余的K-1组子集数据作为训练集，这样会得到K个模型。这K个模型分别在验证集中评估结果，最后的误差MSE(Mean Squared Error)加和平均就得到交叉验证误差。交叉验证有效利用了有限的数据，并且评估结果能够尽可能接近模型在测试集上的表现，可以做为模型优化的指标使用。  
# (option) n_split : データをいくつに分けるかを指定するもの。defaultは3。なお、検定はここで指定した数値の数だけ繰り返される。
# (option) shuffle : defaultはFalseだが、これをTrueにすることで、連続する数字の単純なグループ分けではなく、データセットの中からランダムに値を持ってきてグループを作ることができる。
# (option) random_state : 乱数制御のパラメータでこれを数値にすることで、毎回同じデータセットが得られる。
kf = KFold(n_splits = 5, shuffle = True, random_state = 1)  

# cross_validate
# Evaluate metric(s) by cross-validation and also record fit/score times.  
# https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html
# ------parameter------
# estimator (lr) : the object to use to fit the data
# X (env_data) : the data to fit
# y (price_data) : the target variable to try to predict in the case of supervised learning
# cv : determines the cross-validation splitting strategy
# scoring : a single str or a callable to evaluate the predictions on the test set (https://scikit-learn.org/stable/modules/model_evaluation.html#scoring-parameter)
# ------return------
# test_score : The score array for test scores on each cv split
# fit_time : The time for fitting the estimator on the train set for each cv split
# score_time : The time for scoring the estimator on the test set for each cv split
scores = cross_validate(lr, env_data, price_data, cv = kf, scoring = {"neg_mean_squared_error", "neg_mean_absolute_error"})  
print(scores)  

```

***

## Hyperparameter
https://www.codexa.net/hyperparameter-tuning-python/
* Grid Search
* Random Search
* Bayesian Optimization


***

## 分類問題 - scikit-learnの活用
### 分類アルゴリズムの選択
```python
from sklearn import datasets
from sklearn.model_selection import train_test_split
import numpy as np

iris = datasets.load_iris()
X = iris.data[:,[2,3]]    # 3,4列目の特徴量の抽出
y = iris.target    # クラスラベルを取得
print('Class labels:', np.unique(y))    # 一意なクラスラベルを出力

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=1, stratify=y)
# stratify=y : サブセットに含まれているクラスラベルの比率が入力データセットと同じであること。確認↓
print('Labels counts in y_test:', np.bincount(y_test))

from sklearn.preprocessing import StandardScaler
sc = StandardScaler()
# トレーニングデータの平均と標準偏差を計算
sc.fit(X_train)
# 平均と標準偏差を用いて標準化
X_train_std = sc.transform(X_train)
X_test_std = sc.transform(X_test)

```
