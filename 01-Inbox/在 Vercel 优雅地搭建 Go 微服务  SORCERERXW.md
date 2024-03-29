# 在 Vercel 优雅地搭建 Go 微服务 | SORCERERXW

source:: https://sorcererxw.com/articles/vercel-go-microservice

![Cover](https://sorcererxw.com/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fog-image.vercel.app%252FRuntimes.png&w=3840&q=75)

-   [编写代码](https://sorcererxw.com/articles/vercel-go-microservice#%E7%BC%96%E5%86%99%E4%BB%A3%E7%A0%81)
    -   [基础用法](https://sorcererxw.com/articles/vercel-go-microservice#%E5%9F%BA%E7%A1%80%E7%94%A8%E6%B3%95)
    -   [自定义路由](https://sorcererxw.com/articles/vercel-go-microservice#%E8%87%AA%E5%AE%9A%E4%B9%89%E8%B7%AF%E7%94%B1)
    -   [Monorepo](https://sorcererxw.com/articles/vercel-go-microservice#monorepo)
-   [基础设施](https://sorcererxw.com/articles/vercel-go-microservice#%E5%9F%BA%E7%A1%80%E8%AE%BE%E6%96%BD)
    -   [RPC](https://sorcererxw.com/articles/vercel-go-microservice#rpc)
    -   [定时任务](https://sorcererxw.com/articles/vercel-go-microservice#%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1)
    -   [日志监控](https://sorcererxw.com/articles/vercel-go-microservice#%E6%97%A5%E5%BF%97%E7%9B%91%E6%8E%A7)
    -   [数据库](https://sorcererxw.com/articles/vercel-go-microservice#%E6%95%B0%E6%8D%AE%E5%BA%93)
    -   [Integrations](https://sorcererxw.com/articles/vercel-go-microservice#integrations)
-   [总结](https://sorcererxw.com/articles/vercel-go-microservice#%E6%80%BB%E7%BB%93)

Vercel 云函数是 AWS lambda 的一层封装，提供了非常简单的接入方式，几乎完全免费，对于个人开发者非常友好。在 Node.js 之外，Vercel 还提供了 Go、Python、Ruby 的云函数支持。最近尝试将部分原来部署在 K8S 的个人项目 Go 服务后端迁移到 Vercel，本文将介绍一下这一过程中踩过的坑。

Vercel 云函数定义方式非常简单，使用代码文件路径作为云函数的 url path，只需要在对应目录下放上函数文件即可。对于 Go 来说，则是定义一个 Go 源码文件，如 `api/user/api.go` 则用于处理所有 path 为 `/api/user` 的请求，并需要在其中暴露一个 http.Handler 的函数。因为函数名不限制具体名称，甚至可以使用俄文的大写字母来作为函数名，避免被自己的代码调用。

```
package api

func Ф(w http.ResponseWriter, r *http.Request) {
  // do something
}
```

正如上面提到 Vercel 使用代码路径来区分云函数。这样的模式不同于常规的路由注册方式，每增加一个接口就要创建一个目录，无法集中管理，不灵活且不好维护。

如果让所有请求都进入到这一个函数中来呢？我们可以基于 Vercel 提供的路由机制来覆盖原先的路由机制，只需要在服务根目录中建立 `vercel.json` 文件，并配置：

```
{
  "routes": [
    {
      "src": "/.*",
      "dest": "/api/index.go"
    }
  ]
}
```

这样一来，无论什么路径的请求，都会直接调用 `api/index.go` 了。当不再依赖 Vercel 的路由，我们需要自己负责请求的路由。如下面，使用 echo 定义了一个 http.Handler，并使用此 Handler 直接处理所有请求，这样就将云函数调用和传统的开发模式打通了。

```
import (
  "net/http"

  "github.com/labstack/echo/v4"
)

var srv http.Handler

func init() {
   ctl := NewController()
   e := echo.New()
   e.GET("/books",ctl.ListBooks)
   e.POST("/books",ctl.AddBooks)
   srv = e
}

func Ф(w http.ResponseWriter,r *http.Request) {
   srv.ServeHTTP(w,r)
}
```

不过话说回来，让一个路由 handle 所有请求有一个比较大的问题，就是无论多么边缘的接口调用，我们都需要初始化整个系统，这本身与 Serverless 模式存在冲突；另外一方面，最终打包出来的整体可执行文件体积也会比较大。所以更好的办法是实践 DDD 的设计，为不同领域单独暴露云函数，不同领域之间可以通过 RPC 的方式来互相访问。这样，每一个云函数本身只会包含和初始化与自己领域相关的代码。

可以参考 Vercel 对于函数冷启动的文档说明：

[

Conceptual Model for Serverless Functions - Vercel Documentation

Cloud infrastructure providers typically give us access to several layers of abstraction that map to the traditional counterparts: Set of Computers → Cluster Computer → VM Instance (e.g., EC2) Process → Container Thread → Function Vercel focuses on the last one: the Serverless Function.

![icon](https://sorcererxw.com/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fassets.vercel.com%252Fimage%252Fupload%252Ffront%252Ffavicon%252Fvercel%252Ffavicon.ico%3Ftable%3Dblock%26id%3D4e7cc49c-4f8d-4905-9b7b-86828a97729d%26cache%3Dv2&w=32&q=75)

https://vercel.com/docs/serverless-functions/conceptual-model#cold-and-hot-boots





](https://vercel.com/docs/serverless-functions/conceptual-model#cold-and-hot-boots)

编写微服务的时候往往会有公共代码（比如 RPC 定义），拆分为独立的外部包会降低开发效率，这个时候我们可以通过 monorepo 机制将多个服务编写在同一个仓库内，并直接调用共享代码。可以参考我之前的文章：[Golang 在即刻后端的实践](https://sorcererxw.com/articles/jike-golang)

而 Vercel 同样支持 monorepo，可以配置不同子目录作为 project root。不过不同于常规的 Golang monorepo，Vercel 需要 project root 包含 go.mod 文件，而不能使用父级目录下的共享 go.mod。这样，我们就需要为各个目录建立独立的 go.mod：

```
.
├── pkg: 共享代码
│   ├── util
│   ├── ...
│   └── go.mod
├── app
│   ├── app1
│   │   ├── api
│   │   └── go.mod
│   ├── app2
│   │   ├── api
│   │   └── go.mod
│   └── ...
└── ...
```

并且在服务的 go.mod 内通过 `replace` 的方式引入共享代码：

```
module github.com/sorcererxw/demo/app/app1

go 1.16

require (
  github.com/sorcererxw/demo/pkg v1.0.0
)

replace github.com/sorcererxw/demo/pkg => ../../pkg
```

![image](https://sorcererxw.com/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F6ca60151-3221-45e1-b14f-52382a69a497%252FUntitled.png%3Ftable%3Dblock%26id%3Dc7c4f173-808e-4948-9afa-95e12080cc57%26cache%3Dv2&w=3840&q=75)

同时需要为 project 勾选上 `Include source files outside of the Root Directory in the Build Step` ，以确保构建环境能够访问父目录的共享文件。

不同于 AWS Lambda 提供现成的基础设施，Vercel 的大量基础设施都需要开发者自己想办法。

考虑到原生 gRPC 对于 HTTP 不是非常友好，而 Vercel 本身只能通过 http.Handler 暴露接口，所以我选择 Twitch 公司开源的 [twirp](https://github.com/twitchtv/twirp) 作为 RPC 组件。相比 gRPC，twirp 原生兼容 HTTP mux 以及 JSON 序列化，更加适合 Vercel 云函数这样的场景。

目前 Vercel 还不支持实例之间通过 VPC 网络互相访问，服务间只能通过公网互相访问，在性能上会有一定的损耗，安全机制也需要开发者自己保证。更多信息可以参考下面这个 issue：

[

\[Feature Request\]: Allow AWS VPC Peering · Discussion #3684 · vercel/vercel

You can't perform that action at this time. You signed in with another tab or window. You signed out in another tab or window. Reload to refresh your session. Reload to refresh your session.

![icon](https://sorcererxw.com/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fgithub.com%252Ffavicon.ico%3Ftable%3Dblock%26id%3D2cd2d4d8-9d8f-4c18-a058-448c983ba568%26cache%3Dv2&w=32&q=75)

https://github.com/vercel/vercel/discussions/3684





](https://github.com/vercel/vercel/discussions/3684)

由于云函数无法常驻运行，无法在代码内调度定时任务。目前 Vercel 并未原生支持 CronJob 功能，不过我们可以通过 Github Action 完成 CronJob。只需要在 Github Action 配置定时任务，任务内容为定时调用指定云函数接口。

```
on:
  schedule:
    - cron: '0 * * * *'
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: curl -X GET https://example.vercel.app/api/cronjob
```

服务监控涉及到很多方面，不过最基础的就是运行时日志。Vercel 不会保留所有运行日志，只会在开启 Log Drain 之后才会输出日志。如果想要长期监控日志，需要使用第三方服务主动读取 Vercel 的 Log Drain。

我选用了 BetterStack 家的 [Logtail](https://logtail.com/) 服务，可以一键集成到 Vercel 的所有 project 上。1GB/month 和保留3天的免费额度，对于初期的小项目也足够使用了。

![image](https://sorcererxw.com/_next/image?url=https%3A%2F%2Fwww.notion.so%2Fimage%2Fhttps%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252Ffae5d115-77b8-46fe-b6be-34b15cb8e80e%252FUntitled.png%3Ftable%3Dblock%26id%3D39661f9a-faad-4e76-8ae2-f63c31836e9b%26cache%3Dv2&w=3840&q=75)

常规情况下，我们连接数据库（如 MySQL/MongoDB）需要建立 TCP 长连接，这对于一次性的云函数是高成本且没有必要的。所以现在世面上有不少 Cloud Database 服务，通过暴露 Restful RPC 给使用者来操作数据库，避免直接与数据库连接，加快了云函数启动。

我的服务主要使用 MySQL 存储，选用了 [PlanetScale](https://planetscale.com/) 的服务，使用他们封装好的 MySQL Driver 可以与原生的 sql 组件无缝结合。

```
import (
  "database/sql"
  "github.com/go-sql-driver/mysql"
  "github.com/planetscale/planetscale-go/planetscale"
  "github.com/planetscale/planetscale-go/planetscale/dbutil"
)


func NewDB() (*sql.DB, error) {
  client, err := planetscale.NewClient(
    planetscale.WithServiceToken(TokenName, Token),
  )
  if err != nil {
    return nil, err
  }
  mysqlConfig := mysql.NewConfig()
  mysqlConfig.ParseTime = true
  sqldb, err = dbutil.Dial(context.Background(), &dbutil.DialConfig{
      Organization: Org,
      Database:     DB,
      Branch:       Branch,
      Client:       client,
      MySQLConfig:  mysqlConfig,
  })
  if err != nil {
    return nil, err
  }
  return sqldb
}
```

除了良好的接口，PlanetScale 还提供了 MySQL schema 多分支管理，可以在开发分支升级表结构，等一切完毕正常之后再将开发分支的改动在生产分支上一并同步。总的来说，PlanetScale 使用起来还是非常顺手的。

Vercel 最近上线了 [integration 市场](https://vercel.com/integrations)，遴选了不少适合在云函数上使用的外部服务，很大程度上补足了很多空白。就像 [Unbundling AWS](https://www.tclauson.com/2019/09/11/Unbundling-AWS.html) 当中讲到的一样，AWS 中每一个功能都能单独成为一个 Saas 服务，而 Vercel 则是专注于优化云函数运行时，其他组件由更加专业的服务来提供。

相比起自建 K8S 集群或者 AWS lambda，将服务以云函数的方式部署在 Vercel，可以降低很多的维护成本（金钱或者时间）。但是从上面可以看到，Vercel 本身配套的功能有限，绝大多数的基础设施还是需要依赖第三方服务，每多引入一个第三方服务，系统的可靠性可能会下一个台阶。不过，对于个人项目而言，这是可以接受的，未来还会部署更多的服务到 Vercel 上。
