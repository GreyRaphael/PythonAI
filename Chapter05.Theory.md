# Chapter

## Introduction

Unpredictable reason:
- randomness: 比如掷骰子、不确定原理
- chaos
- reflectivity: 股市中行为影响预测结果
- network effect
- history dependency

人工智能三个阶段:
> <img src="res_05/ai_generation.png" height=600>
- 符号学派(rule-bases systems): 重视逻辑，忽略知识
- 控制学派(classic machine learning)：重视知识，无法学习
- 连接学派(representation learning)：可以自主学习

Classic Machine Learning
- Decision Tree
- Logistic regression
- Bayes
- PCA: principal components analysis, 无监督学习

AI Classification
> <img src="res_05/ai_classification.png" height=500>

Machine Learning:
- supervised learning(biggest part):
  - KNN family: 
    - notion of supervise learning
    - square
    - structureed regression
    - bias-variance tradeoff
  - linear regression:
    - linear assumption: gaussian mixture(2vsN) assumption, comapre KNN with linear regression
    - what is feature
    - feature selection:
      - correction
      - distribution-skewness
      - automatic selection
    - lasso & ridge regression:
      - lasso-sparse property
      - when the features are not independent
    - variance-bias paradox
  - linear classification:
    - LDA
    - logistic regression
    - vc dimension, statistical learning theory
    - model vs data complexity
  - neural network:
    - nonlinear
    - universal function approximator
  - SVM:
    - laplace transform
    - nonlinear transform
    - example
  - Native Bayes
  - Graphic models
  - model selection
  - decision tree:
    - tree
      - depth
      - type of data
    - random forest
    - gradient boosting, extreme gradient boosting
- unsupervised learning
- reinforcement learing

## Application

- document classification: 文本分类
- entertainment: netflix
- vision
- speech recognition
- machine translation
- ai designer: 不同脱衣服就可以换衣服
- ai artist
- ai business detection
- finnancial ai report: HFT(high frequency trading), 智能投顾
- agriculture ai
- medicine ai
- detecing earthquake
- game: alpha go

## Bayes & Stochastic Process

- Bayes Analysis:
  - 条件概率
    - 主观概率
    - 幸存者偏差
  - 贝叶斯公式
  - 贝叶斯统计
  - 贝叶斯决策
  - 朴素贝叶斯

Stochastic Process:
- 随机变量
- 各类分布函数
- wiener process
- possson process
- levy process
- monte cario simulation
- 马尔科夫链
- 稳态过程

