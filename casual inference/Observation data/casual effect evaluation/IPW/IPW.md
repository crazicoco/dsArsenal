## 使用场景
基于倾向分的一种评估干预因果效应的方法，与PSM的使用场景类似，但是不需要根据倾向分匹配，采用加权的方法即可有效评估。另不需要筛选样本，采用全样本加权处理，有效评估ATE（估计全体人群的平均处理效应）


## 假设




## IPW详细步骤
- 使用逻辑回归、随机森林等方法估计每个个体的倾向得分
- 计算权重：
通过权重调整，消除处理分配与协变量之间的关联，模拟随机化实验的环境。

    处理组：$w = 1/ps$
    
    控制组：$w = 1 / (1-ps)$

- 加权分析，使用加权后的样本进行效应估计

步骤解释如下：

```python
import statsmodels.api as sm
import numpy as np

# 假设已有数据框df，包含T（处理）、Y（结果）、X（协变量）
# 步骤1：估计倾向得分
ps_model = sm.Logit(df['T'], sm.add_constant(df[X])  # X为协变量列表
ps_result = ps_model.fit(disp=0)
df['ps'] = ps_result.predict(sm.add_constant(df[X]))

# 步骤2：计算ATE权重
df['weight'] = np.where(df['T'] == 1, 1/df['ps'], 1/(1 - df['ps']))

# 步骤3：截断极端权重（可选）
df['weight'] = np.clip(df['weight'], a_min=0.1, a_max=10)

# 步骤4：加权效应估计（均值差）
att_weighted = np.sum(df[df['T']==1]['Y'] * df[df['T']==1]['weight']) / np.sum(df[df['T']==1]['weight'])
control_weighted = np.sum(df[df['T']==0]['Y'] * df[df['T']==0]['weight']) / np.sum(df[df['T']==0]['weight'])
ate = att_weighted - control_weighted
print(f"ATE估计值: {ate:.2f}")

```

## Code
详细的code实现参考[IPW实战.ipynb](https://github.com/crazicoco/dsArsenal/blob/main/casual%20inference/Observation%20data/casual%20effect%20evaluation/IPW/IPW%E5%AE%9E%E6%88%98.ipynb),该实现是基于lalonde数据集，采用causallib.estimation.IPW 实现。


## Reference
参考deepseek