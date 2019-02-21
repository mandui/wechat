# 开发环境准备

## 账号建立
[测试号申请](https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login)，这里建的是一个测试号，里面各种api都可以使用，可以任意玩耍。最好收藏一个地址，有时候入口不好找。

## 环境设置

### 服务器交互
首先了解一下公众号的数据走向，如下图所示。用户的信息先传给微信服务器，然后微信服务器根据设置，找到对应的应用服务器，进行数据交互。使用的是http/https请求。

![server-image](server.png)

### 接入设置 之 概述
设置中需要指明自己的服务器接入点，并提供一个token。

服务器url需要满足：a. 非IP，即有真实域名；b. 不带端口；c. 支持http或https连接。token随意。

![token-image](url_token.png)

这里微信服务器需要确认，提供的这个服务器能确认消息是否是从微信服务器发出的，这需要在自己的服务器端写一个简单校验的功能。

微信例子里提供了一个php版本的例子，这里附上一个[python版本](main.py)的，在服务器端用如下命令即可满足校验功能。

```
python main.py 80
```

### 接入设置 之 Heroku

由于作者之前从未有过服务器端的开发经验，所以开始时就想避免使用真实（工作）环境，这里就会有一个问题，需要找一个真实域名的服务器，提供这样一个后台服务。

目前云平台们都纷纷提供了免费的服务器试用，可以拿到公网IP，但这有两个问题：一是只有公网IP是不能配置成功的，而且国内域名需要备案，流程复杂漫长，域名还要花钱，不适合练手；二是代码开发...也许我没找到正确的方式上传代码，但用putty登腾讯云的命令行敲代码确实挺痛苦。

#### Heroku
Heroku是一个部署网络应用的平台，相当于一个云后端，里面集成了运行环境需要的所有组件包括各类库、数据库、协议支持等等，主要面向不想自己部署服务器的企业，可以租用不同响应规模的产品。

简单来说，用以下配置，即可成功。

```
URL：https://young-anchorage-86960.herokuapp.com/expr-wechat
Token：mandui
```

之后，可以选择：一，用nginx等将这个地址映射到本地，然后本地开服务器，即可完成本地调试过程；二，注册一个Heroku账户，完成startup步骤，获得一个真实网络接口。本地调试会在后面一个section讲，这里简单写一下Heroku的配置。

1. [注册账户](https://signup.heroku.com/login)：也可以使用i-mandui@outlook.com/mandui@2019登录。

2. 准备工作：Heroku的代码使用git提交的，也就是说，可以在本地写好，用git传到服务器，然后Heroku帮你运行。所以有以下工具需要安装：

[Git](https://desktop.github.com/)：Github Desktop集成git命令行和GUI。
[Heroku Cli](https://devcenter.heroku.com/articles/heroku-cli)：Heroku命令行，用于本地远端代码部署。在链接中有详细说明。需要Git支持。

3. [Startup](https://devcenter.heroku.com/start)：Heroku支持多种语言部署，选一个自己熟悉的。Heroku需要识别特定的结构配置来自动化部署网络服务，所以建议第一回跟着startup教程，部署一个默认project，之后再做修改。每一种语言里面描述都很详细，照着做就好。

4. [代码修改](https://devcenter.heroku.com/articles/getting-started-with-java#push-local-changes)：Startup之后，本地应该有一份代码，修改逻辑之后，用以下命令将更改更新到远端，并开启服务：

```
$ git add *
$ git commit -m "some description"
$ git push heroku master
$ heroku open
```

5. [Log](https://devcenter.heroku.com/articles/getting-started-with-java#view-logs)：可以在命令行这样查看打印日志。

### 接入设置 之 映射本地调试

这一步是为了本地开发调试使用的。
