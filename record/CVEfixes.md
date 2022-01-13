CVEfixes报告

# 环境配置

2022.1.10

## Git clone

ssh密钥对: 保存在”.ssh”

```
ssh-keygen -t rsa -C "youremail@example.com"
```

git clone: 对应ssh链接

```
git clone git@github.com:secureIT-project/CVEfixes.git
```

## anaconda环境

创建

conda env create -f environment.yml

### 问题: conflict

```
Collecting guesslang~=2.0]()

 Downloading guesslang-2.2.0-py3-none-any.whl (2.5 MB)

 Downloading guesslang-2.0.3-py3-none-any.whl (2.1 MB)

 Downloading guesslang-2.0.1-py3-none-any.whl (2.1 MB)

 Downloading guesslang-2.0.0-py3-none-any.whl (13.0 MB)

INFO: pip is looking at multiple versions of <Python from Requires-Python> to determine which version is compatible with other requirements. This could take a while.

INFO: pip is looking at multiple versions of pydriller to determine which version is compatible with other requirements. This could take a while.

 

The conflict is caused by:

  guesslang 2.2.1 depends on tensorflow==2.5.0

  guesslang 2.2.0 depends on tensorflow==2.5.0

  guesslang 2.0.3 depends on tensorflow==2.5.0

  guesslang 2.0.1 depends on tensorflow==2.2.0

  guesslang 2.0.0 depends on tensorflow==2.2.0

 

To fix this you could try to:

\1. loosen the range of package versions you've specified

\2. remove package versions to allow pip attempt to solve the dependency conflict

 

Pip subprocess error:

ERROR: Cannot install -r /home/yang/Documents/CVE/CVEfixes/condaenv.ko814pto.requirements.txt (line 2) because these package versions have conflicting dependencies.

ERROR: ResolutionImpossible: for help visit https://pip.pypa.io/en/latest/user_guide/#fixing-conflicting-dependencies

 

CondaEnvException: Pip failed
```



### 解决1(失败): 采用作者的.frozen.yml

in addition, we provide "frozen" versions that list the actual versions at the time of development as respectively requirements.frozen.txt and environment.frozen.yml

```
conda env create -f environment.frozen.yml
```

报错: 应该是需要自己导入包

```
Collecting package metadata (repodata.json): done

Solving environment: failed

 

ResolvePackageNotFound:

 \- cryptography==3.4.7=py39ha2c9959_0

 \- nbconvert==6.0.7=py39h6e9494a_3

 \- zstd==1.4.9=h582d3a0_0

 \- mysql-common==8.0.23=h694c41f_2
```

(未操作) 将not found的package放入pip中

### 解决2(失败):删除指定的版本号, 选择让pip自动决定

```
- pip:

  \- PyDriller~=2.0

  \- guesslang~=2.0
```

删除`guesslang`的版本号

```
- pip:

  \- PyDriller~=2.0

  \- guesslang
```

报错

```
Collecting guesslang

 Using cached guesslang-2.2.0-py3-none-any.whl (2.5 MB)

 Using cached guesslang-2.0.3-py3-none-any.whl (2.1 MB)

 Using cached guesslang-2.0.1-py3-none-any.whl (2.1 MB)

 Using cached guesslang-2.0.0-py3-none-any.whl (13.0 MB)

 Downloading guesslang-0.9.3-py3-none-any.whl (3.2 MB)

 Downloading guesslang-0.9.1-py3-none-any.whl (3.2 MB)

INFO: pip is looking at multiple versions of <Python from Requires-Python> to determine which version is compatible with other requirements. This could take a while.

INFO: pip is looking at multiple versions of pydriller to determine which version is compatible with other requirements. This could take a while.

 

The conflict is caused by:

  guesslang 2.2.1 depends on tensorflow==2.5.0

  guesslang 2.2.0 depends on tensorflow==2.5.0

  guesslang 2.0.3 depends on tensorflow==2.5.0

  guesslang 2.0.1 depends on tensorflow==2.2.0

  guesslang 2.0.0 depends on tensorflow==2.2.0

  guesslang 0.9.3 depends on tensorflow==1.7.0rc1

  guesslang 0.9.1 depends on tensorflow==1.1.0
```

与上面报错内容相同, 都是不同的guesslang依赖不同的TensorFlow, 目前没搞懂原理

发现GitHub的guesslang有相同的issue(https://github.com/yoeo/guesslang/issues/56)

 

我认为的原因: 

Collecting的guesslang的包有好几个, 包括2.2.1, 2.2.0, 2.0.3等, 依赖的TensorFlow版本不同, 所以才会conflicting

 

### 解决3(失败):在base内可以成功安装guesslang

 

举例: pip3安装guesslang

```
pip3 install guesslang
```

首先自动安装TensorFlow

```
collecting guesslang

 Using cached guesslang-2.2.1-py3-none-any.whl (2.5 MB)

Collecting tensorflow==2.5.0

 Downloading tensorflow-2.5.0-cp38-cp38-manylinux2010_x86_64.whl (454.4 MB)

   |████████████████████████████████| 454.4 MB 23 kB/s 

Collecting tensorboard~=2.5

 Downloading tensorboard-2.7.0-py3-none-any.whl (5.8 MB)

   |████████████████████████████████| 5.8 MB 3.2 MB/s
(略)
```

再次运行创建环境的指令, 需要将原本的环境文件删除

![img](file:///C:/Users/ywh/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

![img](file:///C:/Users/ywh/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

报错: 与之前相同

 

### 解决4(错误):将guesslang放入dependencies中

思路: 既然已经完成了base中的安装

解决: 将guesslang放入dependencies中

```
dependencies:

 \- python~=3.8

 \- pandas~=1.2

 \- numpy~=1.19

 \- requests~=2.24

 \- PyGithub~=1.54

 \- jupyter

 \- seaborn

 \- matplotlib

 \- guesslang

 \- pip

 \- pip:

  \- PyDriller~=2.0
```

运行后报错:

```
Collecting package metadata (repodata.json): done

Solving environment: failed

 

ResolvePackageNotFound:

 \- guesslang
```

### 解决5(错误):将guesslang~=2.2.1放入pip中

报错

```
Pip subprocess error:

\ ERROR: Could not find a version that satisfies the requirement tensorflow==2.5.0 (from guesslang) (from versions: 2.8.0rc0)

ERROR: No matching distribution found for tensorflow==2.5.0

failed

CondaEnvException: Pip failed
```

先将guesslang注释掉

在之后进行安装

Pip3安装-不指定版本

```
pip3 install guesslang

Collecting guesslang

 Using cached guesslang-2.2.1-py3-none-any.whl (2.5 MB)

 Using cached guesslang-2.2.0-py3-none-any.whl (2.5 MB)

 Using cached guesslang-2.0.3-py3-none-any.whl (2.1 MB)

 Using cached guesslang-2.0.1-py3-none-any.whl (2.1 MB)

 Using cached guesslang-2.0.0-py3-none-any.whl (13.0 MB)

 Using cached guesslang-0.9.3-py3-none-any.whl (3.2 MB)

 Using cached guesslang-0.9.1-py3-none-any.whl (3.2 MB)

Collecting numpy<1.13,>=1.12

 Using cached numpy-1.12.1.zip (4.8 MB)

 Preparing metadata (setup.py) ... done

ERROR: Cannot install guesslang==0.9.1, guesslang==0.9.3, guesslang==2.0.0, guesslang==2.0.1, guesslang==2.0.3, guesslang==2.2.0 and guesslang==2.2.1 because these package versions have conflicting dependencies.

 

The conflict is caused by:

  guesslang 2.2.1 depends on tensorflow==2.5.0

  guesslang 2.2.0 depends on tensorflow==2.5.0

  guesslang 2.0.3 depends on tensorflow==2.5.0

  guesslang 2.0.1 depends on tensorflow==2.2.0

  guesslang 2.0.0 depends on tensorflow==2.2.0

  guesslang 0.9.3 depends on tensorflow==1.7.0rc1

  guesslang 0.9.1 depends on tensorflow==1.1.0

 

To fix this you could try to:

\1. loosen the range of package versions you've specified

\2. remove package versions to allow pip attempt to solve the dependency conflict

 

ERROR: ResolutionImpossible: for help visit https://pip.pypa.io/en/latest/user_guide/#fixing-conflicting-dependencies
```

Pip3安装-指定版本

```
pip3 install guesslang~=2.2.1

Collecting guesslang~=2.2.1

 Using cached guesslang-2.2.1-py3-none-any.whl (2.5 MB)

ERROR: Could not find a version that satisfies the requirement tensorflow==2.5.0 (from guesslang) (from versions: 2.8.0rc0)

ERROR: No matching distribution found for tensorflow==2.5.0
```

### 解决6(失败):channel增加pipy

问题: 为什么在bash中成功安装, 在CVEfixes中无法成功安装?

猜测: channel的问题, 在yml中添加channel = pipy

报错: 与之前相同, 仍然是conflicts

解决: 加上版本号2.1.0

```
name: CVEfixes

channels:

 \- conda-forge

 \- defaults

 \- pypi

dependencies:

 \- python~=3.8

 \- pandas~=1.2

 \- numpy~=1.19

 \- requests~=2.24

 \- PyGithub~=1.54

 \- jupyter

 \- seaborn

 \- matplotlib

 \- pip

 \- pip:

  \- PyDriller~=2.0

  \- guesslang~=2.2.1


```

报错: 

```
Pip subprocess error:

ERROR: Could not find a version that satisfies the requirement tensorflow==2.5.0 (from guesslang) (from versions: 2.8.0rc0)

ERROR: No matching distribution found for tensorflow==2.5.0

failed 

CondaEnvException: Pip failed
```

 

### 解决7(成功):先安装TensorFlow

尝试首先安装TensorFlow

```
(CVEfixes) yang@yangwenhao:~/Documents/CVE/CVEfixes$ conda install tensorflow
```

然后再用pip3安装guesslang, 成功

```
(CVEfixes) yang@yangwenhao:~/Documents/CVE/CVEfixes$ pip3 install guesslang

Collecting guesslang

 Using cached guesslang-2.2.1-py3-none-any.whl (2.5 MB)

Collecting tensorflow==2.5.0

 Downloading tensorflow-2.5.0-cp39-cp39-manylinux2010_x86_64.whl (454.4 MB)

   |████████████████████████████████| 454.4 MB 16 kB/s       

Requirement already satisfied: google-pasta~=0.2 in /home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages (from tensorflow==2.5.0->guesslang) (0.2.0)

Requirement already satisfied: termcolor~=1.1.0 in /home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages (from tensorflow==2.5.0->guesslang) (1.1.0)

Requirement already satisfied: opt-einsum~=3.3.0 in /home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages (from tensorflow==2.5.0->guesslang) (3.3.0)

Collecting six~=1.15.0

 Downloading six-1.15.0-py2.py3-none-any.whl (10 kB)

Collecting tensorflow-estimator<2.6.0,>=2.5.0rc0

 Using cached tensorflow_estimator-2.5.0-py2.py3-none-any.whl (462 kB)

Requirement already satisfied: flatbuffers~=1.12.0 in /home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages (from tensorflow==2.5.0->guesslang) (1.12)

Requirement already satisfied: keras-preprocessing~=1.1.2 in /home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages (from tensorflow==2.5.0->guesslang) (1.1.2)
```



# 运行代码

## 尝试运行代码

### 第一次运行, 报错Key Error

原因是: 访问了不在dict中的key

猜测: 没有把cve list下载下来

```
(CVEfixes) yang@yangwenhao:~/Documents/CVE/CVEfixes$ sh Code/create_CVEfixes_from_scratch.sh

Traceback (most recent call last):

 File "/home/yang/Documents/CVE/CVEfixes/Code/collect_projects.py", line 243, in <module>

  cve_importer.import_cves()

 File "/home/yang/Documents/CVE/CVEfixes/Code/cve_importer.py", line 146, in import_cves

  df_cwes = extract_cwe()

 File "/home/yang/Documents/CVE/CVEfixes/Code/extract_cwe_record.py", line 29, in extract_cwe

  cwefile = cwe_zip.extract("cwec_v4.4.xml", cf.DATA_PATH)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/zipfile.py", line 1616, in extract

  return self._extract_member(member, path, pwd)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/zipfile.py", line 1655, in _extract_member

  member = self.getinfo(member)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/zipfile.py", line 1429, in getinfo

   raise KeyError(

KeyError: "There is no item named 'cwec_v4.4.xml' in the archive"
```



### 直接第二次点击运行,不做修改

```
(CVEfixes) yang@yangwenhao:~/Documents/CVE/CVEfixes$ sh Code/create_CVEfixes_from_scratch.sh

01/10/2022 12:10:11 WARNING: The cve table already exists, loading and continuing extraction...

Traceback (most recent call last):

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 703, in urlopen

  httplib_response = self._make_request(

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 449, in _make_request

  six.raise_from(e, None)

 File "<string>", line 3, in raise_from

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 444, in _make_request

  httplib_response = conn.getresponse()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/http/client.py", line 1377, in getresponse

  response.begin()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/http/client.py", line 320, in begin

  version, status, reason = self._read_status()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/http/client.py", line 289, in _read_status

  raise RemoteDisconnected("Remote end closed connection without"

http.client.RemoteDisconnected: Remote end closed connection without response

 

During handling of the above exception, another exception occurred:

 

Traceback (most recent call last):

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/requests/adapters.py", line 440, in send

  resp = conn.urlopen(

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 785, in urlopen

  retries = retries.increment(

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/util/retry.py", line 550, in increment

  raise six.reraise(type(error), error, _stacktrace)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/packages/six.py", line 769, in reraise

  raise value.with_traceback(tb)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 703, in urlopen

  httplib_response = self._make_request(

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 449, in _make_request

  six.raise_from(e, None)

 File "<string>", line 3, in raise_from

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/urllib3/connectionpool.py", line 444, in _make_request

  httplib_response = conn.getresponse()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/http/client.py", line 1377, in getresponse

  response.begin()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/http/client.py", line 320, in begin

  version, status, reason = self._read_status()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/http/client.py", line 289, in _read_status

  raise RemoteDisconnected("Remote end closed connection without"

urllib3.exceptions.ProtocolError: ('Connection aborted.', RemoteDisconnected('Remote end closed connection without response'))

 

During handling of the above exception, another exception occurred:

 

Traceback (most recent call last):

 File "/home/yang/Documents/CVE/CVEfixes/Code/collect_projects.py", line 245, in <module>

  store_tables(get_ref_links())

 File "/home/yang/Documents/CVE/CVEfixes/Code/collect_projects.py", line 81, in get_ref_links

  unfetched_urls = filter_urls(unique_urls)

 File "/home/yang/Documents/CVE/CVEfixes/Code/collect_projects.py", line 35, in filter_urls

  code = requests.head(url).status_code

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/requests/api.py", line 102, in head

  return request('head', url, **kwargs)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/requests/api.py", line 61, in request

  return session.request(method=method, url=url, **kwargs)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/requests/sessions.py", line 529, in request

  resp = self.send(prep, **send_kwargs)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/requests/sessions.py", line 645, in send

  r = adapter.send(request, **kwargs)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/site-packages/requests/adapters.py", line 501, in send

  raise ConnectionError(err, request=request)

requests.exceptions.ConnectionError: ('Connection aborted.', RemoteDisconnected('Remote end closed connection without response'))
```



### 更改: 将所有的cwec_v4.4改为4.6

Cwec_v4.6即最新版

成功下载

![img](file:///C:/Users/ywh/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

 

![img](file:///C:/Users/ywh/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

 

报错信息: type error

```shell
(CVEfixes) yang@yangwenhao:~/Documents/CVE/CVEfixes$ sh Code/create_CVEfixes_from_scratch.sh

--- Logging error ---

Traceback (most recent call last):

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/logging/__init__.py", line 1083, in emit

  msg = self.format(record)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/logging/__init__.py", line 927, in format

  return fmt.format(record)

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/logging/__init__.py", line 663, in format

  record.message = record.getMessage()

 File "/home/yang/anaconda3/envs/CVEfixes/lib/python3.9/logging/__init__.py", line 367, in getMessage

  msg = msg % self.args

TypeError: not all arguments converted during string formatting

Call stack:

 File "/home/yang/Documents/CVE/CVEfixes/Code/collect_projects.py", line 243, in <module>

  cve_importer.import_cves()

 File "/home/yang/Documents/CVE/CVEfixes/Code/cve_importer.py", line 121, in import_cves

  cf.logger.warning('Reusing', year, 'CVE json file that was downloaded earlier...')

Message: 'Reusing'

Arguments: (2022, 'CVE json file that was downloaded earlier...')
```

更改: 把year的遍历改成current year而不是+1

![img](file:///C:/Users/ywh/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

不顶用, 报错信息相同

 

结论: 先老老实实把代码逻辑搞明白吧…

 

# 小插曲

2022.1.11

## 远程连接的Ubuntu的屏幕抖动

尝试: 更改Windows远程连接中的设置等, 未能解决

经重启解决问题

Reboot

## Ubuntu与Windows共享文件夹

尚未解决

暂时安装了Samba, 并add user to Samba share

不知为何, 在local network share时会出现error没有权限

## 远程桌面经常断开

可能是校园网HIT-WLAN的问题

下载team viewer 但是无法登录, 查找原因, 可能是无法在中国连接服务器, 作罢

哎, 心累

这种不是特别影响工作的事情就先罢了, 太影响心情了