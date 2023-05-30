### 修复腾讯云 DNS 无法调用 --update 2023.1.3
   [API 2.0下线通知](https://cloud.tencent.com/document/product/1278/82311) By github@z0z0r4
   
### 新增支持Actions自选更新V4或V6 ——update 2022.12.19
> 使用方法

   1. 修改 **`.github/workflows/run.yml`**

   2. 新增secret **`DOMAINSV6`**
### 新增支持华为云DNS ——update 2022.10.25
> 使用方法

   1. 安装依赖 **`pip install -r requirements.txt`**

   2. 修改配置文件 **`DNS_SERVER`** **`SECRETID`**  **`SECRETKEY`** **`REGION_HW`**

### 新增优选IPv6功能 ——update 2022.07.06
> 使用方法

​	更新代码，修改脚本中的 `TYPE` 参数即可

### 新增默认线路记录 ——update 2021.12.15

如果需要使用默认线路，请将默认线路的cname记录移除或改为其他线路

默认：DEF

境外：AB


### 功能介绍

筛选出优质的Cloudflare IP（目前在暂不开源，以接口方式提供15分钟更新一次），并使用域名服务商提供的API解析到不同线路以达到网站加速的效果（目前只完成DNSPod和阿里云DNS，后续如果有需求将会加入其他运营商的）。


### 适用人群

1. 小站长，网站经常被打或网站放置在国外需要稳定且速度相对快的CDN
2. 服务器在国外但是想建站的小伙伴
3. 科学上网加速，拯救移动线路（未测试）

### 使用方法

>  必要条件: 
>
>  ★ Cloudflare自选IP并已接入到DNSPod或阿里云DNS，
>
>  ★ Python3、pip环境

#### 方法一：在自己的VPS或电脑中运行（推荐）

1. 安装运行脚本所需依赖

```python
pip install -r requirements.txt
```

2. 登录[腾讯云后台](https://console.cloud.tencent.com/cam/capi)或者[阿里云后台](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.11.2c6a2fbdh13O53),获取 SecretId、SecretKey，如果使用阿里云DNS，注意需要添加DNS控制权限**AliyunDNSFullAccess**

3. 将脚本下载到本地修改cf2dns.py中的SecretId、SecretKey

4. 修改脚本中域名配置信息，可配置多个域名和多个子域名，注意选择DNS服务商

5. 运行程序，如果能够正常运行可以选择cron定时执行(建议15分钟执行一次)

```python
python cf2dns.py
```



#### 方法二：GitHub Actions 运行

1. 登录[腾讯云后台](https://console.cloud.tencent.com/cam/capi)或者[阿里云后台](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.11.2c6a2fbdh13O53),获取 SecretId、SecretKey，如果使用阿里云DNS，注意需要添加DNS控制权限**AliyunDNSFullAccess**

2. Fork本项目到自己的仓库![fork.png](https://img.hostmonit.com/images/2020/11/05/fork.png)

3. 进入第二步中Fork的项目，点击Settings->Secrets and variables-> Actions -> New repository secret，分别是DOMAINS，KEY，SECRETID，SECRETKEY。

   > - DOMAINS  需改域名信息，填写时注意不要有换行  例如：`{"hostmonit.com": {"@": ["CM","CU","CT"], "shop": ["CM", "CU", "CT"], "stock": ["CM","CU","CT"]},"4096.me": {"@": ["CM","CU","CT"], "vv":["CM","CU","CT"]}}`
   > - DOMAINSV6 如果需要更新AAA解析请增加此secrets，格式同DOMAINS。
   > - KEY      API密钥，使用这个KEY `o1zrmHAF` 
   > - SECRETID  第一部中从[腾讯云后台](https://console.cloud.tencent.com/cam/capi)或者[阿里云后台](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.11.2c6a2fbdh13O53),获取到的 `SECRETID  `
   > - SECRETKEY  第一部中从[腾讯云后台](https://console.cloud.tencent.com/cam/capi)或者[阿里云后台](https://help.aliyun.com/document_detail/53045.html?spm=a2c4g.11186623.2.11.2c6a2fbdh13O53),获取到的 `SECRETKEY`

   ![secret.png](https://img.hostmonit.com/images/2023/03/04/actions.png)

4. 修改您项目中的 `cf2dns_actions.py`文件中的`AFFECT_NUM`和`DNS_SERVER`参数，继续修改`.github/workflows/run.yml` 文件，定时执行的时长(建议15分钟执行一次)，最后点击 `start commit` 提交即可在Actions中的build查看到执行情况，如果看到 `cf2dns` 执行日志中有 `CHANGE DNS SUCCESS` 详情输出，即表示运行成功。**需要注意观察下次定时是否能正确运行，有时候GitHub Actions 挺抽风的**

   ![modify.png](https://img.hostmonit.com/images/2020/11/05/modify.png)


   ![commit.png](https://img.hostmonit.com/images/2020/11/05/commit.png)

   

   ![build.png](https://img.hostmonit.com/images/2020/11/05/build.png)

### 免责声明

> 1.网络环境错综复杂，适合我的不一定适合你，所以尽量先尝试免费的KEY或者购买试用版的KEY
>
> 2.有什么问题和建议请提issue或者Email我，不接受谩骂、扯皮、吐槽
>
> 3.为什么收费？ 这个标价我也根本不指望赚钱，甚至不够我国内一台VDS的钱。
>
> ★ 如果当前DNSPod有移动、联通、电信线路的解析将会覆盖掉

