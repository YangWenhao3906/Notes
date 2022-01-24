链接:

[FACT 项目主页](https://fkie-cad.github.io/FACT_core/)

[GitHub Repo](https://github.com/fkie-cad/FACT_core)

[我的fork](https://github.com/YangWenhao3906/FACT_core)

# FACT README

The Firmware Analysis and Comparison Tool (FACT)固件分析与比较工具

固件分析和比较工具(以前称为 Fraunhofer 的固件分析框架(FAF))旨在自动化大多数固件分析过程, 可以

- 解压缩任意固件文件并处理多个分析。
- 比较多个图像或单个文件。
- 分解、分析和比较基于插件，保证了最大的灵活性和可扩展性。

![image-20220122190751442](images/FACT/image-20220122190751442.png)

# 环境配置

## Pre_Install

运行

```shell
$ sudo apt update && sudo apt upgrade && sudo apt install git
$ git clone https://github.com/fkie-cad/FACT_core.git ~/FACT_core
$ ~/FACT_core/src/install/pre_install.sh && sudo mkdir /media/data && sudo chown -R $USER /media/data
```

## WSL only

要确保登录到 WSL 计算机时启动 docker

我用的并不是, Linux安装Docker后会自动运行

![image-20220122203129858](images/FACT/image-20220122203129858.png)

不更改

![image-20220122204012090](images/FACT/image-20220122204012090.png)

## Install

很疑惑,这是怎么回事

### 报错: common helper process没有找到

为什么common helper process没有找到

![image-20220122205350247](images/FACT/image-20220122205350247.png)

明明完成了安装

<img src="images/FACT/image-20220122205747804.png" alt="image-20220122205747804" style="zoom:67%;" />

<img src="images/FACT/image-20220122205832734.png" alt="image-20220122205832734" style="zoom: 67%;" />

### 尝试: 手动安装

不懂,索性手动一条条用pip3 install

<img src="images/FACT/image-20220123193756807.png" alt="image-20220123193756807" style="zoom:67%;" />

## 从头:在conda虚拟环境中配置

### 报错git clone

运行install.py后报错

```shell
(FACT) yang@yangwenhao:~/FACT_core$ src/install.py
[2022-01-23 19:37:14][install][INFO]: FACT Installer 1.2
[2022-01-23 19:37:14][common][INFO]: Updating system
[sudo] yang 的密码： 
[2022-01-23 19:37:19][install][INFO]: Installing apt-transport-https autoconf automake build-essential git libtool python3 python3-dev unzip libfuzzy-dev libmagic-dev
[2022-01-23 19:37:20][common][INFO]: Installing python3 pip
[2022-01-23 19:40:53][install][ERROR]: Pip package git+https://github.com/fkie-cad/common_helper_process.git could not be installed:
  Running command git clone -q https://github.com/fkie-cad/common_helper_process.git /tmp/pip-req-build-jg4rfgyh
  error: RPC 失败。curl 16 Error in the HTTP2 framing layer
  fatal: 远端意外挂断了
ERROR: Command errored out with exit status 128: git clone -q https://github.com/fkie-cad/common_helper_process.git /tmp/pip-req-build-jg4rfgyh Check the logs for full command output.

Traceback (most recent call last):
  File "/home/yang/FACT_core/src/install.py", line 183, in <module>
    install()
  File "/home/yang/FACT_core/src/install.py", line 151, in install
    install_fact_components(args, distribution, none_chosen, skip_docker)
  File "/home/yang/FACT_core/src/install.py", line 165, in install_fact_components
    common(distribution)
  File "/home/yang/FACT_core/src/install/common.py", line 49, in main
    install_pip_packages(PIP_DEPENDENCIES)
  File "/home/yang/FACT_core/src/helperFunctions/install.py", line 265, in install_pip_packages
    run_cmd_with_logging(command, silent=True)
  File "/home/yang/FACT_core/src/helperFunctions/install.py", line 222, in run_cmd_with_logging
    raise err
  File "/home/yang/FACT_core/src/helperFunctions/install.py", line 216, in run_cmd_with_logging
    subprocess.run(cmd_, stdout=PIPE, stderr=PIPE, encoding='UTF-8', shell=shell, check=True, **kwargs)
  File "/home/yang/anaconda3/envs/FACT/lib/python3.9/subprocess.py", line 528, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['sudo', '-EH', 'pip3', 'install', '-U', 'git+https://github.com/fkie-cad/common_helper_process.git', '--prefer-binary']' returned non-zero exit status 1.
```

可能是网络问题,再运行一次

### 再次运行: 报错No module named 'pip'

这次报错很奇怪: ModuleNotFoundError: No module named 'pip', 我明明有pip

```shell
(FACT) yang@yangwenhao:~/FACT_core$ src/install.py
[2022-01-23 19:45:21][install][INFO]: FACT Installer 1.2
[2022-01-23 19:45:21][common][INFO]: Updating system
[2022-01-23 19:45:24][install][INFO]: Installing apt-transport-https autoconf automake build-essential git libtool python3 python3-dev unzip libfuzzy-dev libmagic-dev
[2022-01-23 19:45:25][common][INFO]: Installing python3 pip
[2022-01-23 19:45:30][install][ERROR]: Pip package testresources could not be installed:
Traceback (most recent call last):
  File "/usr/bin/pip3", line 5, in <module>
    from pip._internal.cli.main import main
ModuleNotFoundError: No module named 'pip'

Traceback (most recent call last):
  File "/home/yang/FACT_core/src/install.py", line 183, in <module>
    install()
  File "/home/yang/FACT_core/src/install.py", line 151, in install
    install_fact_components(args, distribution, none_chosen, skip_docker)
  File "/home/yang/FACT_core/src/install.py", line 165, in install_fact_components
    common(distribution)
  File "/home/yang/FACT_core/src/install/common.py", line 49, in main
    install_pip_packages(PIP_DEPENDENCIES)
  File "/home/yang/FACT_core/src/helperFunctions/install.py", line 265, in install_pip_packages
    run_cmd_with_logging(command, silent=True)
  File "/home/yang/FACT_core/src/helperFunctions/install.py", line 222, in run_cmd_with_logging
    raise err
  File "/home/yang/FACT_core/src/helperFunctions/install.py", line 216, in run_cmd_with_logging
    subprocess.run(cmd_, stdout=PIPE, stderr=PIPE, encoding='UTF-8', shell=shell, check=True, **kwargs)
  File "/home/yang/anaconda3/envs/FACT/lib/python3.9/subprocess.py", line 528, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['sudo', '-EH', 'pip3', 'install', '-U', 'testresources', '--prefer-binary']' returned non-zero exit status 1.
```

### 手动安装

<img src="images/FACT/image-20220123200346436.png" alt="image-20220123200346436" style="zoom:67%;" />

