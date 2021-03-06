﻿# Text-Classification

## 语言
Python3.5<br>
## 依赖库
pandas=0.21.0<br>
numpy=1.13.1<br>
scikit-learn=0.19.1<br>
gensim=3.2.0<br>
jieba=0.39<br>

## 方法介绍
### 文本转稀疏矩阵：sentence_2_sparse.py
先用jieba分词，再提供两种稀疏矩阵转换方式：1.转one-hot形式的矩阵，使用pandas的value_counts计数后转dataframe；2.sklearn.feature_extraction.text转成哈希表结构的矩阵。<br>
``` python
from sentence_transform.sentence_2_sparse import sentence_2_sparse
# train_data: 训练集
# test_data: 测试集
# language: 语种
# hash: 是否转哈希存储
# hashmodel: 哈希计数的方式
train_data = ['全面从严治党',
              '国际公约和国际法',
              '中国航天科技集团有限公司']
test_data = ['全面从严测试']
m, n = sentence_2_sparse(train_data=train_data, test_data=test_data, hash=True)
```
![ex1](https://github.com/renjunxiang/Text-Classification/blob/master/picture/sentence_2_sparse.png)

### 文本转词向量：sentence_2_vec.py
先用jieba分词，再调用gensim.models的word2vec计算词向量。<br>
``` python
from sentence_transform.sentence_2_vec import sentence_2_vec
# train_data: 训练集
# test_data: 测试集
# size: 词向量维数
# window: word2vec滑窗大小
# min_count: word2vec滑窗内词语数量
train_data = ['全面从严治党',
              '国际公约和国际法',
              '中国航天科技集团有限公司']
test_data = ['全面从严测试']
train_data, test_data = sentence_2_vec(train_data=train_data,
                                       test_data=test_data,
                                       size=5,
                                       window=5,
                                       min_count=1)
```
![ex2](https://github.com/renjunxiang/Text-Classification/blob/master/picture/sentence_2_vec.png)

## 模型训练
### 监督学习：supervised_classify.py
利用sentence_transform.py文本转稀疏矩阵后，通过sklearn.feature_extraction.text模块转为哈希格式减小存储开销，然后通过常用的机器学习分类模型如SVM和KNN进行学习和预测。本质为将文本转为稀疏矩阵作为训练集的数据，结合标签进行监督学习。<br>
![ex3](https://github.com/renjunxiang/Text-Classification/blob/master/picture/文本分类.png)
### 非监督学习：LDA.py
利用sentence_transform.py文本转稀疏矩阵后，对稀疏矩阵进行ALS分解，转为文本-主题矩阵*主题-词语矩阵。<br>
![ex4](https://github.com/renjunxiang/Text-Classification/blob/master/picture/文本主题分类数据.png)
![ex5](https://github.com/renjunxiang/Text-Classification/blob/master/picture/文本主题分类.png)

## DEMO
### 监督学习的范例：demo_score.py
读取数据集（商业数据暂时保密），拆分数据为训练集和测试集，通过supervised_classify.py进行机器学习，再对每条文本打分。<br>
训练数据已更新,准确率最高84%<br>
![ex6](https://github.com/renjunxiang/Text-Classification/blob/master/picture/demo_score_1.png)
图片为不同数据处理和不同模型的准确率<br>
![ex7](https://github.com/renjunxiang/Text-Classification/blob/master/picture/demo_score_2.png)

### 监督学习+打标签的范例：demo_topic_score.py
读取数据集NLP\data\，关键词：keyword.json，训练集train_data.json<br>，名称的配置文件config.py。然后通过supervised_classify.py对每个主题进行机器学习，再对每条文本打分。<br>
因为没有数据，我自己随便造了几句，训练效果马马虎虎~
![ex8](https://github.com/renjunxiang/Text-Classification/blob/master/picture/文本分类+打标签.png)




