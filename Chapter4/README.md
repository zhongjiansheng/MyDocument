# 第四章：Pytorch

## Pytorch各种类包

1. torch.Tensor
2. torch.nn.Module
3. nn.Parameter
4. autograd.Function
5. torch.optim(存放各种优化函数)
6. torchvision.dataset
7. torchvision.transforms

---

## 基本网络构建

1. 代码实例

   ```python
   import torch
   import torch.nn as nn
   import torch.nn.functional as F
   class Net(nn.Module):
       def __init__(self):
           super(Net,self).__init__()
           self.conv1=nn.Conv2d(1,6,3)
           self.conv2=nn.Conv2d(6,16,3)
           self.fc1=nn.Linear(16*6*6,120)
           self.fc2=nn.Linear(120,84)
           self.fc3=nn.Linear(84,10)
       
       def forward(self,x):
           x=F.max_pool2d(self.conv1(x),2)
           x=F.max_pool2d(self.conv2(x),2)
           x=x.view(-1,self.get_featuresize(x))
           x=F.relu(self.fc1(x))
           x=F.relu(self.fc2(x))
           x=self.fc3(x)
           return x
           
       def get_featuresize(self,x):
           size=x.size()[1:]
           count=1;
           for i in size:
               count*=i
           return count
   net=Net()
   print(net)
   print(net.parameters())
   #output[0]:Net(
   #output[0]: (conv1): Conv2d(1, 6, kernel_size=(3, 3), stride=(1, 1))
   #output[0]: (conv2): Conv2d(6, 16, kernel_size=(3, 3), stride=(1, 1))
   #output[0]: (fc1): Linear(in_features=576, out_features=120, bias=True)
   #output[0]: (fc2): Linear(in_features=120, out_features=84, bias=True)
   #output[0]: (fc3): Linear(in_features=84, out_features=10, bias=True)
   #output[0]:)
   #output[1]:<generator object Module.parameters at 0x7fbd77ffab88>
   params=list(net.parameters())
   print(len(params))
   print(params[1].size())
   #output[0]:10
   #output[1]:torch.Size([6])
   ```

---

## 网络的输入

1. 对于__torch.nn__来说，只支持__批量数据__作为输入

   > 如果你输入的是单个数据，可以通过__input.unsqueeze(0)__来增加一个维度

---

## 损失函数(Loss Function)

1. 代码实例

   ```python
   output=net(input)
   print(output)
   #tensor([[ 0.0782,  0.0443, -0.0680, -0.0350, -0.1153,  0.0264, -0.1377, -0.1792,
   #          0.0409, -0.0135]], grad_fn=<ThAddmmBackward>)
   target=torch.randn(10)
   print(target.size())
   #torch.Size([10])
   target=target.view(1,-1)
   print(target.size())
   #torch.Size([1, 10])
   criterion=nn.MSELoss()
   loss=criterion(output,target)
   print(loss)
   #tensor(1.0684, grad_fn=<MseLossBackward>)
   
   
   loss.backward()
   print(loss.grad_fn)
   #<MseLossBackward object at 0x7f0efa6d5550>
   print(loss.grad_fn.next_functions[0][0])
   #<AddmmBackward object at 0x7f0efa6d5908>
   
   ```

---

## 反向传播（Backprop）

   1. 代码实例
   
      ```python
      net.zero_grad()
      print('conv1.bias.grad before backward')
      print(net.conv1.bias.grad)
      loss.backward()
      
      print('after backward')
      print(net.conv1.bias.grad)
      ```

---

## 修改权重（update the weights）

1. 代码实例

   ```java
   import torch.optim as optim
   optimizer=optim.SGD(net.parameters(),lr=0.01)
   
   optimizer.zero_grad()
   output=net(input)
   loss=criterion(output,target)
   loss.backward()
   optimizer.step()
   ```

---

## 数据相关（What about data?)

1. 不同数据集采用的包

   > Images:Pillow、OpenCV
   >
   > audio:scipy、librosa
   >
   > test:raw python 、Cython

---

## 如何训练一个图片分类(Training an image classifier)

1. 加载并且标准化训练数据集和测试数据集，通过**torchvision(返回的图片格式时PIL格式）**
2. 定义相关网络结构
3. 定义一个损失函数以及优化函数
4. 在数据集上训练一个网络
5. 在测试数据集上测试网络

---

## 在显卡上运行（Training on GPU）

1. 代码实例

   ```python
   device=torch.device("cuda" if torch.cuda.is_available() else "cpu")
   print(device)
   ```

---

## 数据加载和增强

