[codewithzhangyi.com](http://codewithzhangyi.com/2018/06/01/XGBOOST使用说明/ "Malformed request")

## 算法原理简述（基于上面陈天奇的PPT）：
(1) Review of key concepts of supervised learning | 监督学习的主要元素
Y值（label标签）
目标函数（Objective Function）= 损失函数（Loss Function）+ 正则化（Regularization）

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/004.png?raw=true)

* 损失函数表示模型对训练数据的拟合程度，loss越小，代表模型预测的越准。
* 正则化项衡量模型的复杂度，regularization越小，代表模型模型的复杂度越低。
* 目标函数越小，代表模型越好。



(2) Regression Tree and Ensemble | 当你谈决策树时你在谈什么
Tree Ensemble methods的好处：

* Very widely used. Almost half of data mining competition are won by using some variants of tree ensemble methods.
* Invariant to scaling of inputs, so you do not need to do careful features normalization.
* Learn higher order interaction between features. 能够学习到特征间的高维相关性
* Can be scalable, and are used in Industry.
![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/006.png?raw=true)
* 在这页，模型复杂度（function space）是由所有的回归树决定的。
* 学习的是fk（树），而不是权重w——体现gradient的思想。

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/005.png?raw=true)

* 信息增益（Information Gain）：决定分裂节点，主要是为了减少损失loss
* 树的剪枝：主要为了减少模型复杂度，而复杂度被‘树枝的数量’影响
* 最大深度：会影响模型复杂度
* 平滑叶子的值：对叶子的权重进行L2正则化，为了减少模型复杂度，提高模型的稳定性
* 回归树不止用于做回归，还可以做分类、排序等，主要依赖于目标函数的定义


(3) Gradient Boosting (How do we Learn)
* Bias-variance tradeoff is everywhere 偏差与方差的权衡无处不在
* The loss + regularization objective pattern applies for regression tree learning (function learning) 损失+正则的模式适用于回归树学习
* We want predictive and simple functions
预测模型的出路在哪里，结果如下：


![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/007.png?raw=true)

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/008.png?raw=true)

使用二阶泰勒展开式来近似Loss：
![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/009.png?raw=true)

* 箭头所指的就是XGB的目标函数表达式，Obj目标函数 = 损失函数 + 正则项 + 常数项，是个优秀的表达式

## 参数说明
XGB的参数是目前见过的模型里最多的，面试被问到就瞎了，

![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/010.png?raw=true)

https://xgboost.readthedocs.io/en/latest/parameter.html#general-parameters

## XGB的优点
敲桌子！重点考点！
同意义问题：XGB 与 GBDT的区别

1. 损失函数：GBDT是一阶，XGB是二阶泰勒展开
2. XGB的损失函数可以自定义，具体参考 objective 这个参数
3. XGB的目标函数进行了优化，有正则项，减少过拟合，控制模型复杂度
4. 预剪枝：预防过拟合
    * GBDT：分裂到负损失，分裂停止
    * XGB：一直分裂到指定的最大深度（max_depth），然后回过头剪枝。如某个点之后不再正值，去除这个分裂。优点是，当一个负损失(-2)后存在一个正损失(+10)，(-2+10=8>0)求和为正，保留这个分裂。
5. XGB有列抽样/column sample，借鉴随机森林，减少过拟合
6. 缺失值处理：XGB内置缺失值处理规则，用户提供一个和其它样本不同的值，作为一个参数传进去，作为缺失值取值。
    XGB在不同节点遇到缺失值采取不同处理方法，并且学习未来遇到缺失值的情况。
7. XGB内置交叉检验（CV），允许每轮boosting迭代中用交叉检验，以便获取最优 Boosting_n_round迭代次数，可利用网格搜索grid search和交叉检验cross validation进行调参。GBDT使用网格搜索。
8. XGB运行速度快：data事先安排好以block形式存储，利于并行计算。在训练前，对数据排序，后面迭代中反复使用block结构。
    **关于并行，不是在tree粒度上的并行，并行在特征粒度上，对特征进行Importance计算排序，也是信息增益计算，找到最佳分割点。**
9. 灵活性：XGB可以深度定制每一个子分类器
10. 易用性：XGB有各种语言封装
11. 扩展性：XGB提供了分布式训练，支持Hadoop实现
12. 共同优点：
    * 当数据有噪音的时候，树Tree的算法抗噪能力更强
    * 树容易对缺失值进行处理
    * 树对分类变量Categorical feature更友好

### [How does XGBoost do parallel computation?](https://stackoverflow.com/questions/34151051/how-does-xgboost-do-parallel-computation)

Xgboost doesn't run multiple trees in parallel like you noted, **you need predictions after each tree to update gradients.**

**Rather it does the parallelization WITHIN a single tree by using openMP to create branches independently.**

To observe this,build a giant dataset and run with n_rounds=1. You will see all your cores firing on one tree. This is why it's so fast- well engineered.








## 代码实现

### XGB调参
#### 方法一： 直接调参，调用 xgboost包 的 XGBClassifier()
可以对其参数进行手动修改，default参数如下
![](https://github.com/YZHANG1270/Markdown_pic/blob/master/xgboost/025.png?raw=true)

#### 方法二： 随机调参，使用 xgb.cv


```python
best_param = list()
best_seednumber = 123
best_logloss = np.Inf
best_logloss_index = 0

dtrain = xgb.DMatrix(X_train, y_train, feature_names = list(X_train))

# 自定义调参组合------------------------------------
for iter in range(50):
    param = {'objective' : "binary:logistic",            	# 目标函数：logistic的二分类模型，因为Y值是二元的
          	 'max_depth' : np.random.randint(6,11),         # 最大深度的调节范围
          	 'eta' : np.random.uniform(.01, .3),            # eta收缩步长调节范围
          	 'gamma' : np.random.uniform(0.0, 0.2),         # gamma最小损失调节范围
          	 'subsample' : np.random.uniform(.6, .9),             
          	 'colsample_bytree' : np.random.uniform(.5, .8), 
          	 'min_child_weight' : np.random.randint(1,41),
          	 'max_delta_step' : np.random.randint(1,11)}

    cv_nround = 50          # 迭代次数：50
    cv_nfold = 5          # 5折交叉验证
    seed_number = np.random.randint(0，100)
    random.seed(seed_number)

    mdcv <- xgb.cv(params = param, dtrain=dtrain, metrics=["auc","rmse","error","logloss"],
                    nfold=cv_nfold, num_boost_round=cv_nround, verbose_eval = None,
					early_stopping_rounds=8, maximize=False)

    min_logloss = min(mdcv['test-logloss-mean'])
    min_logloss_index = mdcv.index[mdcv[test-logloss-mean] == min(mdcv[test-logloss-mean])][0]

    if min_logloss < best_logloss:
        best_logloss = min_logloss
        best_logloss_index = min_logloss_index
        best_seednumber = seed_number
        best_param = param


random.seed(best_seednumber)
nround = best_logloss_index
print('best_round = %d, best_seednumber = %d' %(nround,best_seednumber))
print('best_param : ------------------------------')
print(best_param)  # 显示最佳参数组合，到后面真正的模型要用
```

#### 方法三：使用 gridsearch 和 cross validation
参考 https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/

(4) 绘制 train/test 的 auc/rmse/error

```python
def xgb_plot(input,output):
  	history=input
  	train_history=history.iloc[:,8:16].assign(id=[i+1 for i in history.index])
  	train_history['Class'] = 'train'
  	test_history=history.iloc[:,0:8].assign(id=[i+1 for i in history.index])
  	test_history['Class'] = 'test'
  	train_history.columns = ["auc_mean","auc_std","error_mean","error_std","logloss_mean","logloss_std","rmse_mean","rmse_std","id","Class"]
  	test_history.columns = ["auc_mean","auc_std","error_mean","error_std","logloss_mean","logloss_std","rmse_mean","rmse_std","id","Class"]
  
  	his=pd.concat([train_history,test_history])

  
  	if output=="auc":
		his['y_min_auc'] = his['auc_mean']-his['auc_std']
		his['y_man_auc'] = his['auc_mean']+his['auc_std']

    	auc=ggplot(his,aes(x='id', y='auc.mean', ymin='y_min_auc', ymax='y_man_auc',fill=Class)+\
    	  geom_line()+\
    	  geom_ribbon(alpha=0.5)+\
    	  labs(x="nround",y='',title = "XGB Cross Validation AUC")
    	return(auc)
    
 
  
  	if output=="rmse":
		his['y_min_rmse'] = his['rmse_mean']-his['rmse_std']
		his['y_man_rmse'] = his['rmse_mean']+his['rmse_std']

    	rmse=ggplot(his,aes(x='id', y='rmse.mean',ymin='y_min_rmse',ymax='y_man_rmse',fill=Class))+\
    	  geom_line()+\
    	  geom_ribbon(alpha=0.5)+\
    	  labs(x="nround",y='',title = "XGB Cross Validation RMSE")
    	return(rmse)

  
  	if output=="error":
		his['y_min_error'] = his['error_mean']-his['error_std']
		his['y_man_error'] = his['error_mean']+his['error_std']

    	error=ggplot(his,aes(x='id',y='error.mean',ymin='y_min_error',ymax='y_man_error',fill=Class))+\
    	  geom_line()+\
    	  geom_ribbon(alpha=0.5)+\
    	  labs(x="nround",y='',title = "XGB Cross Validation ERROR")
    	return(error)
```

横坐标是迭代次数，可以观察迭代时是否过拟合
train曲线和test曲线的相差程度，可以侧面反映模型复杂度，检验是否过拟合
```python
xgb_plot(mdcv,'auc')
xgb_plot(mdcv,'rmse')
xgb_plot(mdcv,'error')

```

### 建模，进行预测，打印评估指标
#### 方法一： 使用 xgboost.train

```python
# 利用上面调参结果： best_param

md_1 = xgb.train(best_param, dtrain, num_boost_round=nround)

# 预测
dtest = xgb.DMatrix(X_test, feature_names=list(X_test))
preds = md_1.predict(dtest)
print(mean_square_error(y_test, preds))

predictions = [round(value) for value in preds]
accuracy = accuracy_score(y_test, predictions)
f1_score = f1_score(y_test,predictions)
print("Accuracy: %.2f%%" %(accuracy * 100.0))
print("F1 Score: %.2f%%" %(f1_score * 100.0))

# save model
md_1.save_model('xgb.model')
```

#### 方法二： 使用 XGBClassifier()

```python
# 由于 xgb.train 与 XGBClassifier() 有部分参数的名字稍有出入，具体参考API接口文档
best_param['learning_rate'] = best_param.pop('eta')  # 修改参数字典的某个key名字
best_param.update({'colsample_bytree': 1})           # 取消列抽样，修改参数字典的某个value

md_2 = XGBClassifier(**best_param)                   # 2个*号，允许直接填入字典格式的param
md_2.fit(X_train, y_train)  

ypred = md_2.predict(X_test)
predictions = [round(value) for value in ypred]

# 打印评估指标
MSE = mean_squared_error(y_test, predictions)
print("MSE: %.2f%%" % (MSE * 100.0))  
accuracy = accuracy_score(y_test, predictions)
print("Accuracy: %.2f%%" % (accuracy * 100.0))
f1_score = f1_score(y_test, predictions)
print("F1 Score: %.2f%%" % (f1_score * 100.0))
```

### 绘制Importance排序图

```python
ax = xgb.plot_importance(md_2, height=0.5)
fig = ax.figure
fig.set_size_inches(25,20)                  # 可调节图片尺寸和紧密程度
plt.show()
```

### 根据Importance进行特征筛选

```python
# sorted(list(selection_model.booster().get_score(importance_type='weight').values()),reverse = True)

importance_plot = pd.DataFrame({'feature':list(X_train.columns),'importance':md_2.feature_importances_})
importance_plot = importance_plot.sort_values(by='importance')
importance_plot = importance_plot.reset.index(drop=True)
thresholds = importance_plot.importance
thresholds_valid = np.unique(thresholds[thresholds != 0])


for thresh in thresholds_valid:

	# select features using threshold
	selection = SelectFromModel(md_2, threshold=thresh, prefit=True)
	select_X_train = selection.transform(X_train)
	# train model
	selection_model = XGBClassifier(**best_param)
	selection_model.fit(select_X_train, y_train)
	# eval model
	select_X_test = selection.transform(X_test)
	y_pred = selection_model.predict(select_X_test)
	predictions = [round(value) for value in y_pred]
	accuracy = accuracy_score(y_test, predictions)
	print("Thresh=%.4f, n=%d, Accuracy: %.2f%%" % (thresh, select_X_train.shape[1], accuracy*100.0))


thresh = 0.034
selected_features = list(importance_plot[importance_plot.importance > thresh]['feature'])
print('selected features are :\n %s'%selected_features)
select_X_train = X_train[selected_features]                        # 筛选Importance符合阈值的特征集

n_features = selected_X_train.shape[1]
print('total: %d features are selected' %n_features)

selection_model = XGBClassifier(**best_param)                                   
selection_model.fit(select_X_train, y_train)

select_X_test = X_test[selected_features]
y_pred = selection_model.predict(select_X_test)
predictions = [round(value) for value in y_pred]
accuracy = accuracy_score(y_test, predictions)
f1_score = f1_score(y_test, predictions)
print("Accuracy: %.2f%%" % (accuracy * 100.0))
print("F1 Score: %.2f%%" % (f1_score * 100.0))
```

至于是先调参，再做变量筛选，还是先筛选后调参，或是反复调参反复筛选，纯凭个人喜号。

### 绘制决策树
```pyhton
# graphviz文件存在的路径配置
os.environ["PATH"] += os.pathsep + 'C:/Users/Yi/Anaconda3/envs/release/bin/'  # 在引号''这里替换你的dot.exe路径
xgb.to_graphviz(md_2, num_trees=0, rankdir='LR')  # num_trees的值是第几棵树，0为第一棵，rankdir是树的方向，default是从上到下

```