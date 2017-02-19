# CodeIgniter 4 路线图 - 第二阶段

> 原文: [CodeIgniter4 Roadmap - Phase 2](http://forum.codeigniter.com/thread-65543.html)

CodeIgniter4 is a rewrite of the framework, using PHP7, with substantial changes and improvements. It is not intended to be backwards-compatible with CodeIgniter3, though we are shooting to ease the pain of transition, where possible.

CodeIgniter4 是一个使用 PHP7 重写的，包含重大更改和改进的框架。虽然并不打算向后兼容 CodeIgniter3，但我们正在尽可能的排除过渡期的痛苦。

CodeIgniter 4 has reached milestone 1, following our [development roadmap](codeigniter-4-proposed-roadmap.md)! Phase 1 has been completed, and we are embarking on Phase 2.

CodeIgniter 4 已经到了 milestone 1，跟随我们的[开发路线图](codeigniter-4-proposed-roadmap.md)！第一阶段已经完成，我们正在着手第二阶段。

The first phase has seen the core re-implemented, with a number of essential components:

第一阶段可以看见核心代码重新实现了，同时也有许多基本的部件：

*  Autoloader (classmap, PSR4 autoloader, and CI magic)
*  Logging (PSR7 compliant)
*  Exception Handling
*  HTTP Request/Response (for input & output)
*  Routing (convention & configuration)
*  Controllers (CI-style)
*  Models (CI-style, Active Record)
*  Database Layer (with adapters for MySQLi and Postgre)
*  Config (flexible, environment)
*  Security (Escaper, CSRF)
*  Sessions (with file & database adapters)
*  Views
*  Debugging and Profiling Tools.

*  自动加载器(classmap，PSR4 自动加载和 CI 魔术)
*  日志（符合 PSR7）
*  异常处理
*  HTTP 请求/应答层（或输入/输出）
*  路由（约定和配置）
*  控制器（CI 风格）
*  模型（CI 风格，活动记录）
*  数据库层（适用于 MySQLi 和 Postgre）
*  配置（灵活性，环境）
*  安全（转义器，CSRF）
*  会话 (适用于文件和数据库)
*  视图
*  调试和分析工具

We are now embarking on phase two:

我们正在着手于第二阶段：

*  helpers
*  Language/Localization
*  Caching
*  Email
*  Encryption
*  Form Validation
*  Image Library
*  Pagination
*  Uploader
*  Sessions (memcached & redis adapters)
*  Database adapters for other RDBs

*  辅助函数
*  语言/本地化
*  缓存
*  Email
*  加密
*  表单验证
*  图像库
*  分页
*  文件上传
*  会话 (适用于memcached和redis)
*  其他关系数据库的适配

The [CodeIgniter4 repository](https://github.com/bcit-ci/CodeIgniter4) has been opened up for community contributions towards milestone 2. We are looking to implement the phase 2 features in a controlled & managed fashion.

[CodeIgniter4 repository](https://github.com/bcit-ci/CodeIgniter4) 已经向社区贡献者开放了 milestone 2。我们正在寻求实现第二阶段的特性在一个受控和管理的方式下。

We would particularly appreciate additions to the unit testing, with a target code coverage of 80+%.

我们特别欣赏添加的单元测试，目标代码覆盖率为80+%。

We are *not* looking for someone's favorite library or package.

我们*不*寻求某个人喜欢的库或包。

We are *not* looking for any shopping cart or authentication packages.

我们*不*寻求任何购物车或认证包。

We are looking for contributions that work, with unit testing, user guide changes, changelog entries, and which conform to our style guide. Please read the [Contributing to CodeIgniter](https://bcit-ci.github.io/CodeIgniter4/contributing) section before diving in!

我们寻求与单元测试, 用户手册的变更, 更新日志，以及符合我们风格的指南的建设。在行动前请阅读[Contributing to CodeIgniter](https://bcit-ci.github.io/CodeIgniter4/contributing) 部分。

We are using issues, flagged as enhancements or features, to manage the ongoing development of the framework.

我们使用 issues，标记作为增强或特性，去管理正在开发的框架。

We are using issues flagged as bugs for, well, bug tracking.

我们使用 issues，标记当做 bug，也是 bug 跟踪。

If you have a feature to propose or discuss, do so on the [CI4 feature request subforum](https://forum.codeigniter.com/forum-29.html). not as github issues.

如果您有一个特性建议或讨论，请在[CI4 feature request subforum](https://forum.codeigniter.com/forum-29.html)发表。而不是 github issues。

The [CI4 Support](https://forum.codeigniter.com/forum-30.html) subforum has been created to help community members with setup,environment and process issues.

 [CI4 Support](https://forum.codeigniter.com/forum-30.html)子论坛已经被创建，用于帮助社区成员解决安装，环境和处理问题。

The [CI4 Discussion](https://forum.codeigniter.com/forum-31.html) has been setup for any other discussions.

同时也设立了[CI4 Discussion](https://forum.codeigniter.com/forum-31.html)供大家讨论其他问题。

We will use this [CI4 Development](https://forum.codeigniter.com/forum-27.html) subforum to report developments to the community.

我们会使用[CI4 Development](https://forum.codeigniter.com/forum-27.html)子论坛向社区报道开发进度。

We are trying to keep the CodeIgniter 4 User Guide in synch with developments. We are shooting for either a nightly build that you have access to, or perhaps a build which is part of a successful travis-ci run. For now, we have setup the new [User Guide](https://bcit-ci.github.io/CodeIgniter4) on github, and it will be updated "frequently".

我们正在努力保持 CodeIgniter 4 用户手册与开发同步。我们正在努力进行一个你可以访问的每夜版本，或者是一个成功运行travis-ci的版本的一部分。现在，我们在github上设置了新的[User Guide](https://bcit-ci.github.io/CodeIgniter4)，它将会被“频繁”更新。
