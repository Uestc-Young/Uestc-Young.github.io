---
title: Pytorch Review
date: 2023-07-27 20:23:25
tags: 
- [Computer Vision]
- [Machine Learning]
categories: [Pytorch]
mathjax: true
typora-root-url: ..
---

本文准备跟着b站`小土堆`的pytorch教程系统地再熟悉一下pytorch的操作，去年这时候就打算跟着`小土堆`的教程来看，结果又鸽了(~~还有个原有就是当时太菜了，根本看不懂~~)，今年决定好好的看完做完。  
建议学时：18h

## Pytorch环境安装

略，本台电脑上面没有`NVIDIA`的显卡，所以在官网安装了pytorch的CPU版本。

## 如何查看包中某些函数如何使用

```python
dir(torch)
help(torch.cuda.is_available)
```

help()函数输出的是官方文档的解释内容

或者直接ctrl+单击到不知道的函数上就行了(前提是你知道这个函数叫啥，建议边学边查文档)

<!--more-->

## Pytorch加载数据

```python
import torch
from torch.utils.data import Dataset
from PIL import Image
import os

dir_path="hymenoptera_data/train"
ants_label_path="ants"
bees_label_path="bees"

class MyData(Dataset):
    def __init__(self,dir_path,label_path):
        self.dir_path=dir_path
        self.label_path=label_path
        self.path=os.path.join(self.dir_path,self.label_path)
        self.img_path=os.listdir(self.path)

    
    def __getitem__(self, index):
        img_name=self.img_path[index]
        img_path_name=os.path.join(self.dir_path,self.label_path,img_name)
        img=Image.open(img_path_name)
        label=self.label_path
        return img,label
    
    def __len__(self):
        return len(self.img_path)
    
my_ants_dataset=MyData(dir_path,ants_label_path)
```

这里加载数据时的label由于和文件夹一样，就无需再另外加载了，如果需要从数据集的label文件夹里面读取label就根据具体的文件比如txt，json，csv等等来进行调整，同时，使用`pandas`库来读取csv文件也是一个不错的选择。由于这里我们是处理图像数据，故选用`PIL`，亦可以使用`cv2`.  
用`cv2`进行图片数据读取的话是这样的：

```python
import cv2
img=cv2.imread(path)
```

## 使用Tensorboard可视化数据

```python
from torch.utils.tensorboard import SummaryWriter

writer = SummaryWriter("logs")

# writer.add_image()
for i in range(100):
    writer.add_scalar("y = 2x",2*i,i)

writer.close()
```

`tensorboard --logdir=logs --port=6008`  
由于我这里指定了要写入logs这个文件夹，只要在Python终端里面输入这样的一行命令就可以了。  

下面是图片可视化

```python
from torch.utils.tensorboard import SummaryWriter
from PIL import Image
import numpy as np

writer = SummaryWriter("logs")
img_path="data\\train\\ants_image\\0013035.jpg"
img_PIL=Image.open(img_path)
img_array=np.array(img_PIL)

writer.add_image("test",img_array,1,dataformats="HWC")

writer.close()
```

注意tensorboard的需要的输入格式只能是torch.tensor或者是numpy.array，所以需要对于PIL读入的数据进行格式转化处理，同时需要注意默认的输入是(Channel，H，W)，这里打印出来(img_array.shape)数据发现不符合原先的形式，需要在后面手动指定数据格式。

## Transforms的使用

```python
from torchvision import transforms
from PIL import Image
img_path="data\\train\\ants_image\\0013035.jpg"
img=Image.open(img_path)

Totensor=transforms.ToTensor()
norm=transforms.Normalize([0.5,0.5,0.5],[0.5,0.5,0.5])

print(Totensor(img))
print(norm(img))
```

要注意的点就是`transforms`内含的诸如`ToTensor`等不能直接使用，它是一个类，需要先进行赋值才能使用。  
这样将打开的图片转化为tensor形式就可以直接输入tensorboard里面了。

```python
from PIL import Image
from torchvision import transforms
from torch.utils.tensorboard import SummaryWriter

dir_path="vectorization\images (only for reference)\image1.jpg"
img=Image.open(dir_path)
resize=transforms.Resize((512,512))
totensor=transforms.ToTensor()
compose=transforms.compose([resize,totensor])

img_resize=resize(img)
img_totensor1=totensor(img)
img_totensor2=totensor(img_resize)

writer = SummaryWriter()

writer.add_image("resize test",img_totensor1,1)
writer.add_image("resize test",img_totensor2,2)


writer.close()
```

一些transforms的运用 ~~你妈的我一导包就是导入transformer~~

## DataLoader使用

```python
import torchvision
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

train_data=torchvision.datasets.CIFAR10("./CIFAR",train=True,transform=torchvision.transforms.ToTensor())
test_data=torchvision.datasets.CIFAR10("./CIFAR",train=False,transform=torchvision.transforms.ToTensor())

test_loader=DataLoader(test_data,batch_size=64,shuffle=True,num_workers=0)

writer = SummaryWriter("logs")
step = 0
for epoch in range(0,2):
    for data in test_loader:
        imgs,targets = data
        writer.add_images("dataloader",imgs,global_step=step)
        step=step+1

writer.close()
```

dataloader就是在自己构造好的数据集基础上，进行多批次数据的读取`batch_size`.`num_workers`就是用来进行多进程并行计算的东西，没试过.`shuffle = True`就表示如果有多个epoch，每次打乱顺序重新读取，否则不打乱。  
这样的话能够结合`tensorboard`很好的进行数据的读取和显示。  

## nn.Module的使用

```python
class mynn(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(in_channels=3,out_channels=6,kernel_size=3,stride=1,padding=0)
    
    def forward(self,x):
        self.conv1(x)
        return x
# Convolution
class mynn(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3,32,5,1,2),
            nn.MaxPool2d(2),
            nn.Conv2d(32,32,5,1,2),
            nn.MaxPool2d(kernel_size=2),
            nn.Conv2d(32,64,5,padding=2),
            nn.MaxPool2d(kernel_size=2),
            nn.Flatten(),
            nn.Linear(1024,64),
            nn.Linear(64,10)
        )

    def forward(self,input):
        output = self.model1(input)
        return output
# 使用Sequential
```

## 损失函数与反向传播

```python
cifar_test = mynn()

loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(cifar_test.parameters(),lr=0.1)

for epoch in range(10):
    epoch_loss = 0
    for data in dataloader:
        img,label = data
        output = cifar_test(img)
        loss = loss_fn(output, label)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        epoch_loss = epoch_loss + loss
    print(f"Epoch {epoch + 1} / 10 , Loss : {epoch_loss} ")
```

顺序是先梯度清零`optimizer.zero_grad()`;  
然后反向传播`loss.backward()`;  
最后更新参数`optimizer.step()`.

## 模型的加载与保存

```python
# 保存训练好的参数
save_path='./weights.pth'
torch.save(net.state_dict(),save_path)
# 导入参数
load_path='./weights.pth'
load_weights=torch.load(load_path)
net.load_state_dict(load_weights)
```

## CIFAR十分类任务训练

```python
import torch
import torchvision
from torch.utils.data import DataLoader
from torch import nn
from torch.utils.tensorboard import SummaryWriter

train_set = torchvision.datasets.CIFAR10("CIFAR",train=True,transform=torchvision.transforms.ToTensor(),download=False)
test_set = torchvision.datasets.CIFAR10("CIFAR",train=False,transform=torchvision.transforms.ToTensor(),download=False)

train_data_length = len(train_set)
test_data_length = len(test_set)

train_dataloader = DataLoader(train_set,batch_size=64)
test_dataloader = DataLoader(test_set,batch_size=64)

# 搭建神经网络
class CIFAR_model(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3,32,5,1,2),
            nn.MaxPool2d(2),
            nn.Conv2d(32,32,5,1,2),
            nn.MaxPool2d(kernel_size=2),
            nn.Conv2d(32,64,5,padding=2),
            nn.MaxPool2d(kernel_size=2),
            nn.Flatten(),
            nn.Linear(1024,64),
            nn.Linear(64,10)
        )

    def forward(self,input):
        output = self.model1(input)
        return output

model = CIFAR_model()

loss_fn = nn.CrossEntropyLoss()

learning_rate = 0.05

optimizer = torch.optim.SGD(model.parameters(),lr=learning_rate)

training_step = 0
test_step = 0

writer = SummaryWriter("logs_CIFAR")

for epoch in range(10):
    print("Epoch {} starts!".format(epoch+1))

    model.train()
    for data in train_dataloader:
        imgs,labels = data
        outputs = model(imgs)
        loss = loss_fn(outputs,labels)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        training_step = training_step + 1
        if training_step % 100 == 0:
            print("Training Steps: {}, Loss: {}".format(training_step,loss.item()))
            writer.add_scalar("Training loss",scalar_value=loss.item(),global_step=training_step)
        
    model.eval()
    total_test_loss = 0
    total_test_accuracy = 0
    with torch.no_grad():
        for data in test_dataloader:
            imgs,labels = data 
            outputs = model(imgs)
            loss = loss_fn(outputs,labels)
            total_test_loss = total_test_loss + loss.item()

            accuracy = (outputs.argmax(1) == labels).sum()
            total_test_accuracy = total_test_accuracy + accuracy

            test_step = test_step + 1
    print("Loss on test dataset:{}".format(total_test_loss))
    print("Total accuracy on test dataset:{}".format(total_test_accuracy / test_data_length))
    writer.add_scalar("Test loss",scalar_value=total_test_loss,global_step=test_step)
    writer.add_scalar("Test accuracy",scalar_value=total_test_accuracy / test_data_length,global_step=test_step)

writer.close()
```

结合使用`tensorboard`，使得训练数据可视化，同时在分类任务中，可以进行准确率的计算.

## 使用GPU训练

由于本人这台电脑上面没有没有`NVIDIA`的GPU，所以就记录一下，反正肯定能用上，~~到时候抄一下代码就行了~~

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
loss_fn = loss_fn.to(device)
model = model.to(device)
imgs = imgs.to(device)
labels = labels.to(device)
```

只有`loss`,`model`,`data`可以使用`.to(device)`的功能.  
没有GPU的可以放到`Google colab`上训练即可!(~~现在体会到肉身翻墙的好处了~~)

## 使用模型进行测试

```python
import torch
import torchvision
from torch.utils.data import DataLoader
from torch import nn, reshape
from PIL import Image

class CIFAR_model(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.model1 = nn.Sequential(
            nn.Conv2d(3,32,5,1,2),
            nn.MaxPool2d(2),
            nn.Conv2d(32,32,5,1,2),
            nn.MaxPool2d(kernel_size=2),
            nn.Conv2d(32,64,5,padding=2),
            nn.MaxPool2d(kernel_size=2),
            nn.Flatten(),
            nn.Linear(1024,64),
            nn.Linear(64,10)
        )

    def forward(self,input):
        output = self.model1(input)
        return output

load_path = 'model_7.pth'
model = torch.load(load_path,map_location=torch.device('cpu'))
# print(model)
img_path = "dog.png"
img = Image.open(img_path)
# print(img)
trans = torchvision.transforms.Compose([
    torchvision.transforms.Resize((32,32)),
    torchvision.transforms.ToTensor()
])
img = trans(img)
# print(img)
# print(img.shape)
img = reshape(img,(1,3,32,32))
model.eval()
with torch.no_grad():
    output = model(img)
print(output.argmax(1))
```

## 完结撒花 and 碎碎念

花了两天左右时间把`b站小土堆`的pytorch入门视频总算是看完了，基本算是熟悉了计算机视觉`Computer Vision`方面的操作流程以及一些技巧，但是调参之类的东西还得靠自己去实践(~~比如最后CIFAR训练的时候梯度就莫名其妙爆炸了~~)。  
由于上学期一直在做`综合设计`项目，因此看了很多自然语言处理`NLP`方面的知识，也算是有些收获吧。还是要多练，多写点代码熟悉一下流程以及一些库的使用。希望下学期能有点时间去打一打`Kaggle`上的竞赛提升一下自己吧。  
马上大三了，也不知道自己以后到底读研的方向是什么，甚至还不太清楚是打算保研还是出国(sigh)。两手准备吧。顺便在拿大三的一些日子来摸索摸索方向了，得想想自己是否真的喜欢某个领域。  

**反正，菜就多练**。  

2023/7/18  
写于`新加坡国立大学(NUS)`
