# 欢迎使用ASGO-GPU平台
本平台运行基于CentOS系统上，使用GridView软件进行任务调度。详细指令可参考[Slurm官方手册](https://slurm.schedmd.com/quickstart.html)。
用户入门请参阅《[用户手册](用户手册.pdf)》。

### 调度平台web入口
打开浏览器，输入：
> 10.69.21.155:6080

可以进行用户账号申请、登录、作业提交、状态查看等操作。

### 节点登录
计算节点放置在校内，需要在校园网环境登录，校外请先连接VPN；
- Linux\Mac用户可直接在终端通过ssh的方式连接集群；
- Windows用户可通过windows terminal、xshell、putty或者mobaxterm连接集群；
> ssh -p 22100 username@10.69.21.155


### 作业提交：
提交作业请先编写作业脚本，并通过`sbatch`命令提交。

假设作业脚本文件名为job.sh，
```bash
#!/bin/bash
#SBATCH -o job.%j.out          #输出文件路径
#SBATCH --partition=operation  #队列名称，目前有debug 和 partition可选；优先级不同
#SBATCH --qos=low
#SBATCH -J myFirstJob          #可以给任务命名
#SBATCH --nodes=1              #申请节点数 1
#SBATCH --gres=gpu:2 #每个节点上申请2个GPU

# your tasks here
hostname
echo 'This is my first job !'
```

则通过以下命令提交：
> chmod 775 job.sh

> sbatch job.sh

作业结束后，可以在web界面或者ssh到登录节点查看结果。

### 环境配置
系统预先配置了部分软件环境，可以通过增加环境变量的方式引用。全局软件放置于'/data1/software/'下。

比如使用预装的conda，只需要将文件位置加入环境变量即可：

> vim .bashrc

增加下面一行指令：

'export PATH=/data1/software/anacinda3/bin:$PATH'

又如调用cuda9.1：

'export PATH=/data1/software/cuda9.1/bin:$PATH'

'export LD_LIBRARY_PATH=/data1/software/cuda9.1/lib64:$LD_LIBRARY_PATH'

然后刷新环境变量

> source .bashrc

环境配置完毕。

#### 示例：

至此，我们已经可以使用系统预先配置的环境运行任务了。
编辑测试脚本'testgpu.slurm'如下：
```
#!/bin/bash
#SBATCH --partition=operation
#SBATCH --qos=low
#SBATCH --nodes=1                 # 申请一个节点
#SBATCH --cpus-per-task=2
#SBATCH --ntasks-per-node=6
#SBATCH --gres=gpu:2            # 每个节点上申请2块GPU卡

#your task here:
python testgpu.py
```
其中，'testgpu.py'为Pytorch官方测试小程序：
```Python
import torch
import math

dtype = torch.float
# device = torch.device("cpu")
device = torch.device("cuda:0") # Uncomment this to run on GPU

# Create random input and output data
x = torch.linspace(-math.pi, math.pi, 2000, device=device, dtype=dtype)
y = torch.sin(x)

# Randomly initialize weights
a = torch.randn((), device=device, dtype=dtype)
b = torch.randn((), device=device, dtype=dtype)
c = torch.randn((), device=device, dtype=dtype)
d = torch.randn((), device=device, dtype=dtype)

learning_rate = 1e-6
for t in range(2000):
    # Forward pass: compute predicted y
    y_pred = a + b * x + c * x ** 2 + d * x ** 3

    # Compute and print loss
    loss = (y_pred - y).pow(2).sum().item()
    if t % 100 == 99:
        print(t, loss)

    # Backprop to compute gradients of a, b, c, d with respect to loss
    grad_y_pred = 2.0 * (y_pred - y)
    grad_a = grad_y_pred.sum()
    grad_b = (grad_y_pred * x).sum()
    grad_c = (grad_y_pred * x ** 2).sum()
    grad_d = (grad_y_pred * x ** 3).sum()

    # Update weights using gradient descent
    a -= learning_rate * grad_a
    b -= learning_rate * grad_b
    c -= learning_rate * grad_c
    d -= learning_rate * grad_d


print(f'Result: y = {a.item()} + {b.item()} x + {c.item()} x^2 + {d.item()} x^3')
```
则我们运行任务如下：
> sbatch testgpu.slurm


### 其他环境配置
如果系统预装环境不能够满足计算需求，则需要大家自己进行相关开发环境配置。

#### 普通环境配置
ssh至admin1节点后，根据软件包提示，安装在用户本地目录即可。

#### GPU环境配置
对于需要使用GPU计算卡的软件包配置，需要多一步操作。
因为ssh登录后admin1节点自身没有GPU卡，需要首先提交一个空任务。空任务排队成功后，即可以登录申请的装有GPU卡的计算节点上，如gpu3节点，然后配置环境。
配置完毕，后期执行任务只需要登录admin1提交任务即可。

步骤1： 提交空任务，等待调度排队
![空任务提交](gpu3.bmp "空任务提交")

步骤2：
排队成功后，登录计算节点，配置GPU环境。

> ssh -p 22100 username@10.69.21.155

> ssh gpu3



<!-- ### 用户交流群
![ASGO-GPU用户群](qr.bmp "限时有效")
 -->

<!-- Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for -->

<!-- ```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/asgogpu/asgogpu.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

 -->
### 参考材料
1. [SLURM官方手册中文版](https://docs.slurm.cn/users/)
1. [SLURM官方手册英文版](https://slurm.schedmd.com/documentation.html)
