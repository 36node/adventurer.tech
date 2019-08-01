# 36node 团队的敏捷&DevOps

我们采用经过简化的敏捷开发方式，并结合持续集成和持续发布，以适合冒险者公会的弹性工作的特点。

1. 强调主观能动性，以及成员员之间的互动，优于既定流程和工具。
2. 软件能够运行，优于详尽的文档。
3. 跟客户的密切协作，优于合同和谈判。
4. 能够响应变化，优于遵循计划。

## 项目团队组成

按照项目或者项目群区分不同的开发团队，每个开发团队包含如下角色：

1. 架构师：项目技术负责人，负责整个项目的技术架构和任务分配。
2. 产品设计：负责和需求方沟通，产出原型图，协调设计资源，推进开发进度，校验产品。
3. 开发：包括全职和兼职开发，用代码实现产品，开发应该完全了解我们团队的 DevOps。

一个项目团队需要完成从产品设计、开发一直到运维部署的全部工作。

## 我们采用的分支模型

采用基于`git-flow`简化的`pull request`分支模型。

1. `master`作为唯一主分支，利用`tag`作为版本管理。
2. 从`master`分支迁出`feature`分支进行新功能开发。
3. 从`master`分支迁出`refactor`分支进行部分重构开发。
4. 从`master`分支迁出`bug`分支进行错误修复。
5. 从`master`分支迁出`release`分支进行版本发布。
6. 从`tag`迁出`hotfix`分支进行临时修复，在`hotfix`分支上直接发布。
7. `Pull Request`简称`PR`，是所有代码进入主分支的唯一渠道。
8. 任何代码进入`master`之前，应该经过充分的自动化测试、代码审阅以及开发环境产品功能校验。

## 环境

1. 开发环境: 搭建在本地机器上，用于开发新功能、修复 bug、发布以及产品校验。
2. UAT 环境: 代表集成测试环境，体现最新的已集成代码，用于产品、客户的整体版本预览和集成测试。
3. 生产环境: 服务于用户的最终部署。

通常开发环境采用本地数据库或者 mock 数据，如果需要调试在线数据，则谨慎使用 UAT 环境的数据。

![环境](https://ws2.sinaimg.cn/large/006tNc79ly1g2h0r2ileaj30hl0g0wf9.jpg)

开发环境是最外层的大圈，按顺时针方向一圈作为一次迭代。每一个迭代的交付物，将进入 UAT 环境。最终我们从 UAT 环境交付产品到达生产环境。

## 迭代流程

一个完整的迭代由架构师发起，架构师根据产品的需求和项目本身的代码改进等，规划一个迭代所需要完成的事情。
为了减轻每次准备的工作量，在工作过程中将未来可能执行的需求，放入 issue 池中，在迭代开启的时候，方便选择。

迭代按两周一个周期？或者每个迭代灵活变动？

1. 迭代前准备，通常在上一个迭代准备下一个迭代。
   1. 产品需要完成初步的原型设计`issue`，否则难以估计工作量，导致迭代破产。
   2. 充分准备的`issue`池。
   3. 任何人都应将发现的问题，需要改进的建议放入`issue`池。
2. 迭代启动会，由架构师主持。 // TODO: 需要做什么？
3. 迭代进行中:
   1. 产品跟进各个`feature`或者`bug`。
   2. 开发领取任务，由 36node 整体协调资源，弹性调配。
   3. 开发代码过程中的`commit`记录可以自动关闭对应的`issue`。
   4. `commit`记录应当简洁准确，压缩冗余提交。
   5. 开发通过`PR`提交任务，代码和 master 有冲突时，采用`rebase`的方式。
   6. 产品在开发环境完成产品功能性校验。
   7. 架构师负责代码审阅，通过 PR。
   8. 开发在`PR`上点击`merge`。
   9. 完善自动化测试。
4. 迭代发布：
   1. 由架构师发起`release`的`PR`，可能涉及多个工程。推送`release tag`。
   2. 产品在 UAT 环境检验本次迭代的产品目标是否达成。
   3. 产品通过`PR`。
   4. 架构师完成合并发布分支。
5. 迭代总结会，由架构师主持。// TODO: 需要做什么？

## DevOps 自动化流程

1. 每个`commit`会跑`unit test`。
2. 每个`PR`会跑`int test`。
3. `release tag`会自动触发镜像打包。
4. UAT 环境关联最新 latest 镜像。

## 域名规则

一个 project 的 domain

```sh
// 部署在 36node
{xxx}.{project}[.36node].com

// 部署在 客户自己域名下
{xxx}[.navinfo].com
```

api 的规则

```sh
api.{project}[.36node].com/service/v0
api.{project}[.36node].com/service/v0-uat
api.{project}[.36node].com/service/v0-pre

api.navinfo.com/some-service/v0
api.pudongbus.36node.com/bus-core/v0
```

我们把前端称为 app, 以 life 和 bus project 为例

```sh
admin.life.36node.com
admin-uat.life.36node.com

// 省略默认的app名称
chongmingbus.36node.com
admin.chongmingbus.36node.com


// admin-uat 是 admin web 的 uat 版本
admin-uat.pudongbus.36node.com

screen.chongmingbus.36node.com
screen-uat.chongmingbus.36node.com
```
