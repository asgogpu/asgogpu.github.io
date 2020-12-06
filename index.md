## 欢迎使用ASGO-GPU平台
本平台运行基于CentOS系统上，使用GridView软件进行任务调度。详细指令可参考[Slurm官方手册](https://slurm.schedmd.com/quickstart.html)。
用户入门请参阅《[用户手册](用户手册.pdf)》。

### 调度平台入口
打开浏览器，输入：
> 10.69.21.155:6080
可以进行用户账号申请、登录、作业提交、状态查看等操作。

### 节点登录
需要在校园网环境登录，校外请先连接VPN； Linux\Mac用户可直接在终端通过ssh的方式连接集群； Windows用户可通过windows terminal、xshell、putty或者mobaxterm连接集群；
> ssh -p 22100 username@10.69.21.155


### 作业提交：
提交作业请先编写作业脚本，并通过`sbatch`命令提交。

假设作业脚本文件名为job.sh，
```bash
#!/bin/bash
#SBATCH -o job.%j.out
#SBATCH --partition=debug #队列名称，目前有debug 和 operation 可选；优先级不同
#SBATCH --qos=low
#SBATCH -J myFirstJob
#SBATCH --nodes=1          #申请节点数
#SBATCH --gres=gpu:TeslaV100-SXM2-32GB:2  #每个节点上申请2个GPU

hostname
echo 'This is my first job !'
```
则通过以下命令提交：
> chmod 775 job.sh

> sbatch job.sh

### 用户交流群
![ASGO-GPU用户群](qr.bmp "限时有效")

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
