# meteor项目部署

meteor项目有多种部署方式，可以选择部署到meteor官方推荐的Galaxy平台，但由于国内网络的原因，这种方式并不适合当下国情。另外，还有两种部署到自有服务器中的方法。

ps:本文章重点讲解mup的部署方式

## mup

mup的部署方式也是当前我所采用的部署方案，其高效的部署过程，使得欲罢不能。但是由于种种原因，导致国内外多数meteor开发者无法使用mup部署，其原因大概如下：

1. 网络原因。通过配置国内源解决这个问题。目前，我的方案是，npm使用淘宝，docker使用daocloud。
2. 版本原因。通过修改meteor项目的版本(1.4及以上)，mup的版本，node(4.4.7)，npm(2.15.8)的版本来解决。
3. dockerImage原因。由于mup部署工具历史原因，选择一个合适的dockerImage，是非常重要的。


下面将重点讲述meteor项目mup的部署方法。(以下方法仅限于meteor 1.4及1.4.x版本)

1. 安装mup，初始化部署文件(见底部)。

2. 配置文件，mup.js，settings.json

   1. `mup.js` 如下

      ```javascript
      module.exports = {
          servers: {
              //服务器配置
              one: {
                  host: 'xxx.xxx.xxx.xxx',
                  username: 'root',
                  password: 'password'
              }
          },
          meteor: {
              name: 'projectName',
              path: 'projectPath',
              servers: {
                  one: {}
              },
              env: {
                  //占用服务端口
                  PORT: 8007,
                  //cdn地址
                  CDN_URL:'xxxx',
                  //根域名地址
                  ROOT_URL: 'http://www.ohuoyi.com',
                  //mongodb 数据库地址
                  MONGO_URL: 'xxx'
              },
              //指定docker image.
              //这个地方是大多数人错误的地方,由于默认的image存在问题,所以必须填写如下的地址
              		
          dockerImage:"abernix/meteord:base",
              //waitTime,由于国内的原因,这个世界尽量大一些.120-200
              deployCheckWaitTime: 120 //default 10
          }
      };
      ```

   2. `settings.json`，如下

      ```
      {
          "public": {
              // 根据需要填写
              // 如下是使用ga统计包含的 id
              "analyticsSettings": {
                  "Google Analytics": {
                      "trackingId": "xxxx"
                  }
              }
          }
      }
      ```

3. 执行相关指令

   1. `mup setup`，成功执行截图如下

      ![](http://resources.ohuoyi.com/mup-setup.png)

   2. `mup deploy`，成功执行截图如下

      ![](http://resources.ohuoyi.com/mup-deploy.png)

4. 顺利完成上述操作后，通过IP:PORT 方式即可访问了，如需加上域名，可选择nginx反向代理。如果使用cdn，在nginx配置中加入cdn，否则可能出现可以访问，但无法使用的现象。

通过上面这种方式，笔者已经成功部署了多个项目，每次部署大概耗时5分钟左右。如果你有问题，可以来我的论坛交流。http://coderapp.ohuoyi.com

## build & deploy

这种方式的其实已经被上述的方式集成化了，这种方式的原理大致如下，

1. 本地打包项目部署文件 `meteor build`
2. 上传到服务器
3. 解压部署文件，执行

由于笔者偷懒，掌握mup的部署方式后，就不在折腾了这种方法了。但是这是不！对！的！希望能折腾的同学使用后与我交流。我们交流的地方无疑还是那个论坛。 http://coderapp.ohuoyi.com



以上

## 附件资料

[meteor官方部署文档](https://guide.meteor.com/deployment.html#deployment-options)

[mup安装](https://github.com/kadirahq/meteor-up)