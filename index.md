## 欢迎使用ASGO-GPU平台
本平台运行基于CentOS系统上，使用GridView软件进行任务调度。详细指令可参考[Slurm官方手册](https://slurm.schedmd.com/quickstart.html)。
用户入门请参阅《[用户手册](用户手册.pdf)》。

### 调度平台入口
打开浏览器，输入：
> 10.69.21.155:6080

### 节点登录
> ssh username@10.69.21.155 port 22100

### 作业提交：
提交作业请先编写作业脚本，并通过`sbatch`命令提交。
```bash
#!/bin/bash
#SBATCH -o job.%j.out
#SBATCH --partition=C032M0128G
#SBATCH --qos=low
#SBATCH -J myFirstJob
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1

hostname
echo 'This is my first job !'
```
假设作业脚本文件名为job.sh，则通过以下命令提交：
>sbatch job.sh

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
