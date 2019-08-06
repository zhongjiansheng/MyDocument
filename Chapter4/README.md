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

1. 代码实例

   ```python
   from __future__ import print_function, division
   
   import torch
   import torch.nn as nn
   import torch.optim as optim
   from torch.optim import lr_scheduler
   import numpy as np
   import torchvision
   from torchvision import datasets, models, transforms
   import matplotlib.pyplot as plt
   import time
   import os
   import copy
   
   plt.ion()   # interactive mode
   
   data_transforms={
       'train':transforms.Compose([
           transforms.RandomResizedCrop(224),
           transforms.RandomHorizontalFlip(),
           transforms.ToTensor(),
           transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
       ]),
       'val': transforms.Compose([
           transforms.Resize(256),
           transforms.CenterCrop(224),
           transforms.ToTensor(),
           transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
       ]),
   }
   data_dir='data/hymenoptera_data'
   image_datasets={x:datasets.ImageFolder(os.path.join(data_dir,x),data_transforms[x])
                  for x in ['train','val']}
   dataloaders={x:torch.utils.data.DataLoader(image_datasets[x],batch_size=4,shuffle=True,
                                             num_workers=4)
               for x  in ['train','val']}
   dataset_size={x:len(image_datasets[x]) for x in ['train','val']}
   class_names=image_datasets['train'].classes
   device=torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
   def imshow(inp, title=None):
       """Imshow for Tensor."""
       inp = inp.numpy().transpose((1, 2, 0))
       mean = np.array([0.485, 0.456, 0.406])
       std = np.array([0.229, 0.224, 0.225])
       inp = std * inp + mean
       inp = np.clip(inp, 0, 1)
       plt.imshow(inp)
       if title is not None:
           plt.title(title)
       plt.pause(0.001)  # pause a bit so that plots are updated
   
   
   # Get a batch of training data
   inputs, classes = next(iter(dataloaders['train']))
   
   # Make a grid from batch
   out = torchvision.utils.make_grid(inputs)
   
   imshow(out, title=[class_names[x] for x in classes])
   ```

---

## 训练模型

1. 代码实例

   ```python
   def train_model(model,criterion,optimizer,scheduler,num_epochs=25):
       start=time.time()
       best_model_wts=copy.deepcopy(model.state_dict())
       best_acc=0.0
       for epoch in range(num_epochs):
           print('Epoch {}/{}'.format(epoch,num_epochs-1))
           print('-'*10)
           for phase in['train','val']:
               if phase=='train':
                   scheduler.step()
                   model.train()
               else:
                   model.eval()
               running_losee=0.0
               running_corrects=0
               for inputs,labels in dataloaders[phase]:
                   inputs=inputs.to(device)
                   labels=labels.to(device)
                   optimizer.zero_grad()
                   with torch.set_grad_enabled(phase=='train'):
                       outpus=model(inputs)
                       _,pred=torch.max(outpus,1)
                       loss=criterion(outpus,labels)
                       
                       if phase=='train':
                           loss.backward()
                           optimizer.step()
                   running_losee+=loss.item()*inputs.size(0)
                   running_corrects += torch.sum(preds == labels.data)
               epoch_loss=running_losee/dataset_size[phase]
               epoch_acc = running_corrects.double() / dataset_sizes[phase]
               print('{} Loss: {:.4f} Acc: {:.4f}'.format(
                   phase, epoch_loss, epoch_acc))
               if phase=='val' and epoch_acc>best_acc:
                   best_acc=epoch_acc
                   best_model_wts=copy.deepcopy(model.state_dict())
           print()
       time_elapsed=time.time()-start
       print('Training complete in {:.0f}m {:.0f}s'.format(time_elapsed//60,time_elapsed%60))
       print('Best val Acc:{:4f}'.format(best_acc))
       model.load_state_dict(best_model_wts)
       return model
   
                       
               
   ```

---

## 可视化模型预测

1. 代码实例

   ```
   def visualize_model(model,num_images=6):
       was_training=model.training
       model.eval()
       images_so_far=0
       fig=plt.figure()
       with torch.no_grad():
           for i,(inputs,labels) in enumerate(dataloaders['val']):
               inputs=inputs.to(device)
               labels=labels.to(device)
               outputs=model(inputs)
               _,pred=torch.max(outputs,1)
               for j in range(inputs.size()[0]):
                   images_so_far+=1
                   ax=plt.subplot(num_images//2,2,images_so_far)
                   ax.axis('off')
                   ax.set_title('predicted:{}'.format(class_names[pred[j]]))
                   imshow(inputs.cpu().data[j])
                   
                   if images_so_far==num_images:
                       model.train(mode=was_training)
                       return
           model.train(mode=was_training)
               
   ```

---

## 微调(Finetuning the convnet)

1. 代码实例

   ```python
   model_ft = models.resnet18(pretrained=True)
   num_ftrs = model_ft.fc.in_features
   model_ft.fc = nn.Linear(num_ftrs, 2)
   
   model_ft = model_ft.to(device)
   
   criterion = nn.CrossEntropyLoss()
   
   # Observe that all parameters are being optimized
   optimizer_ft = optim.SGD(model_ft.parameters(), lr=0.001, momentum=0.9)
   
   # 调整学习率：Decay LR by a factor of 0.1 every 7 epochs
   exp_lr_scheduler = lr_scheduler.StepLR(optimizer_ft, step_size=7, gamma=0.1)
   ```

---

## 固定特定层

1. 代码实例

   ```python
   model_conv = torchvision.models.resnet18(pretrained=True)
   for param in model_conv.parameters():
       param.requires_grad = False
   
   # Parameters of newly constructed modules have requires_grad=True by default
   num_ftrs = model_conv.fc.in_features
   model_conv.fc = nn.Linear(num_ftrs, 2)
   
   model_conv = model_conv.to(device)
   
   criterion = nn.CrossEntropyLoss()
   
   # Observe that only parameters of final layer are being optimized as
   # opposed to before.
   optimizer_conv = optim.SGD(model_conv.fc.parameters(), lr=0.001, momentum=0.9)
   
   # Decay LR by a factor of 0.1 every 7 epochs
   exp_lr_scheduler = lr_scheduler.StepLR(optimizer_conv, step_size=7, gamma=0.1)
   ```

   

