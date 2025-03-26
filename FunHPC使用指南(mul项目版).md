# FunHPC使用指南(mul项目版)

## 租赁并连接VSCode

进入FunHPC，选购一种gpu主机，进入购买页面

镜像需要选择`pytorch/2.3.1/3.10/12.1`

<img src="https://migo-typora.oss-cn-beijing.aliyuncs.com/image-20250326195652446.png" alt="image-20250326195652446" style="zoom: 33%;" />

此处`我的数据`处选择`celeba(3G)`，等待服务器完成启动

<img src="https://migo-typora.oss-cn-beijing.aliyuncs.com/image-20250326200941615.png" alt="image-20250326200941615" style="zoom:33%;" />

在`SSH连接`处取得ssh连接命令和密码，形如

```bash
ssh -p 40579 root@wpuyc8mftogi7f61snow.deepln.com
```

根据该命令修改ssh配置文件

```
Host funhpc
    HostName wpuyc8mftogi7f61snow.deepln.com
    User root
    Port 40579
```

**此处的`Port`和`HostName`字段需要于当前租赁生成的ssh连接命令一致。**

随后可以使用vscode进行连接，过程中需要输入密码。

<img src="https://migo-typora.oss-cn-beijing.aliyuncs.com/image-20250326201603296.png" alt="image-20250326201603296" style="zoom: 33%;" />

（可选）FunHPC貌似已经自带Python等必备扩展，但个人建议额外安装若干VSCode扩展方便开发，如：

- copilot(免费版够用)
- black formatter
- isort
- data wrangler
- git graph
- yaml

## 配置环境

### 下载代码

首先下载代码，可以使用git命令、scp命令或者直接使用vscode拖动上传

git命令（最简单，推荐使用）:

```bash
cd
git clone https://github.com/MigoXV/mul.git
cd mul
```

scp命令:

[github页面](https://github.com/MigoXV/mul#)下载项目`mul.zip`文件后，在该文件所在目录打开powershell

```bash
scp mul.zip funhpc:~
```

`funhpc`为上文ssh配置文件中的`Host`字段

vscode上传：

vscode中打开文件夹后，直接从本地拖拽上传即可。

### 安装依赖

首先在vscode中打开`mul`目录

<img src="https://migo-typora.oss-cn-beijing.aliyuncs.com/image-20250326202648748.png" alt="image-20250326202648748" style="zoom: 33%;" />

<img src="https://migo-typora.oss-cn-beijing.aliyuncs.com/image-20250326202829482.png" alt="image-20250326202829482" style="zoom: 25%;" />

注意，使用VSCode连接FunHPC后打开目录后，终端不会自动跳转到打开的目录，需要手动跳转，下方的命令已经包含

```bash
cd ~/mul # 手动跳转mul目录
pip install -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple poetry # 安装poetry
poetry env use /data/miniconda/envs/torch/bin/python3.10 # 创建poetry虚拟环境
eval $(poetry env activate) # 激活虚拟环境
poetry install # 安装依赖
```

## 启动训练

安装完成后使用如下命令启动训练

```bash
mult --config-dir mul/presets --config-name base_train
```

