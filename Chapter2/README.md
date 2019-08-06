# 第二章:机器学习

---



* `机器学习的三种方法`

  > 监督学习
  >
  > 无监督学习
  >
  > 强化学习

* 感知器实现

  ```python
  import pandas as pd
  import numpy as np
  class Perception(object):
      def __init__(self,eta=0.001,n_iter=10):
          self.eta=eta
          self.n_iter=n_iter
      
      def fit(self,x,y):
          self.w=np.zeros(1+x.shape[1])
          self.errors_=[]
          for i in range(self.n_iter):
              errors=0
              for xi,target in zip(x,y):
                  update=self.eta*(target-self.predic(x))
                  self.w[1:]+=update*xi
                  self.w[0]+=update
                  errors+=int(update!=0.0)
              self.errors_.append(errors)
          return self
                
                  
      def net_input(self,x):
          return np.dot(x,self.w[1:])+self.w[0]
          
      def predic(self,x):
          return np.where(self.net_input(x)>=0.0,1,-1)
  ##数据准备
  df=pd.read_csv('http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data',header=None)
  df.tail()
  ```

  * 基本术语
    1. 超参数：训练前设置的参数（学习率、迭代次数等）
    2. 优化算法 
       1. 批量梯度下降
       2. 随机梯度下降

---

## scikit-learn

