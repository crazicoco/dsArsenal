## 使用场景
Causal Impact，该方法基于合成控制法的原理，利用多个对照组数据来构建贝叶斯结构时间序列模型，并调整对照组和实验组之间的大小差异后构建综合时间序列基线，最终预测反事实结果。


## 假设
满足三大假设：
1. 条件独立性(CIA)，所有干预混淆变量都被观察，不存在未观察的变量；
2. 正值假设：对于任意的X值，每个对象接受干预还是对照的机会一致；
3. 稳定单位处理值假设：单位间具有独立性，不产生相互作用，一种处理对应一种结果。

额外假设：
1. 协变量外生性：协变量 不受干预影响
2. 模型稳定性：干预前的模型结构在干预后保持不变。

## Code
存在包体安装问题，故后续在应用过程中，可以参考 http://yonbviewer.org/github/jamalsenouci/causalimpact/blob/master/GettingStarted.ipynb 

## 参考

1. https://blog.csdn.net/weixin_39293132/article/details/132414701
2. https://nbviewer.org/github/jamalsenouci/causalimpact/blob/master/GettingStarted.ipynb
