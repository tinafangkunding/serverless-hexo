# 部署Serverless Hexo站点

## 快速开始

通过Serverless Website组件快速构建一个Serverless Hexo站点 <p/>
build a serverless hexo website with serverless website component

1. [安装](#1-安装)
2. [配置](#2-配置)
3. [部署](#3-部署)
4. [移除](#4-移除)

### 1. 安装

**安装前提：**
- [Node.js](https://nodejs.org/en/) (Node.js 版本需不低于 8.6，建议使用 Node.js 10.0 及以上版本)
- [Git](https://git-scm.com/)

如您未安装上述应用程序，可以参考排[Hexo安装说明](https://hexo.io/zh-cn/docs/)。

**安装Hexo**
```
$ npm install -g hexo-cli
```
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```
$ hexo init hexo   # 生成hexo目录
$ cd hexo
$ npm install
```

新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

安装完成后，可以通过`hexo g`命令生成静态页面
```
hexo g   # generate
```

> 注：如果希望在本地查看效果，也可以运行下列命令，通过浏览器访问 `localhost:4000` 查看页面效果。
```
hexo s   # server
```

### 2. 配置

在`hexo`目录下，创建`serverless.yml`文件，在其中进行如下配置

```console
$ touch serverless.yml
```

```yml
# serverless.yml

myWebsite:
  component: "@serverless/tencent-website"
  inputs:
    code:
      src: ./localhexo/public # Upload static files generated by HEXO
      index: index.html
      error: index.html
    region: ap-guangzhou
    bucketName: my-bucket
```

配置完成后，文件目录如下：
```
.
├── .serverless
├── hexo
|   ├── public
|   ├── ...
|   ├── _config.yml
|   ├── ...
|   └── source
└── serverless.yml
```

### 3. 部署

通过`sls`命令进行部署，并可以添加`--debug`参数查看部署过程中的信息

如您的账号未[登陆](https://cloud.tencent.com/login)或[注册](https://cloud.tencent.com/register)腾讯云，您可以直接通过`微信`扫描命令行中的二维码进行授权登陆和注册。

```
PS serverless --debug

  DEBUG ─ Resolving the template's static variables.
  DEBUG ─ Collecting components from the template.
  DEBUG ─ Downloading any NPM components found in the template.
  DEBUG ─ Analyzing the template's components dependencies.
  DEBUG ─ Creating the template's components graph.
  DEBUG ─ Syncing template state.
  DEBUG ─ Executing the template's components graph.
  DEBUG ─ Starting Website Component.

Please scan QR code login from wechat
Wait login...
Login successful for TencentCloud
  DEBUG ─ Preparing website Tencent COS bucket my-bucket-1250000000.
  DEBUG ─ Deploying "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ "my-bucket-1250000000" bucket was successfully deployed to the "ap-guangzhou" region.
  DEBUG ─ Setting ACL for "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ Ensuring no CORS are set for "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ Ensuring no Tags are set for "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ Configuring bucket my-bucket-1250000000 for website hosting.
  DEBUG ─ Uploading website files from D:\hexotina\localhexo\public to bucket my-bucket-1250000000.
  DEBUG ─ Starting upload to bucket my-bucket-1250000000 in region ap-guangzhou
  DEBUG ─ Uploading directory D:\hexotina\localhexo\public to bucket my-bucket-1250000000
  DEBUG ─ Website deployed successfully to URL: https://my-bucket-1250000000.cos-website.ap-guangzhou.myqcloud.com.

  myWebsite:
    url: https://my-bucket-1250000000.cos-website.ap-guangzhou.myqcloud.com
    env:

  13s » myWebsite » done
```
访问命令行输出的website url，即可查看您的Serverless Hexo站点

> 注：如果希望更新hexo站点中的文章，需要在本地重新运行`hexo g`进行生成静态页面，再运行`serverless`更新到页面

### 4. 移除

可以通过以下命令移除hexo网站
```console
PS sls remove --debug

  DEBUG ─ Flushing template state and removing all components.
  DEBUG ─ Starting Website Removal.
  DEBUG ─ Removing Website bucket.
  DEBUG ─ Removing files from the "my-bucket-1250000000" bucket.
  DEBUG ─ Removing "my-bucket-1250000000" bucket from the "ap-guangzhou" region.
  DEBUG ─ "my-bucket-1250000000" bucket was successfully removed from the "ap-guangzhou" region.
  DEBUG ─ Finished Website Removal.

  6s » myWebsite » done

```