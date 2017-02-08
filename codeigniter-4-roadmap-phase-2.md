# CodeIgniter 4 路线图 - 第二阶段

> 原文: [CodeIgniter4 Roadmap - Phase 2](http://forum.codeigniter.com/thread-65543.html)

CodeIgniter4 is a rewrite of the framework, using PHP7, with substantial changes and improvements. It is not intended to be backwards-compatible with CodeIgniter3, though we are shooting to ease the pain of transition, where possible.

CodeIgniter 4 has reached milestone 1, following our [development roadmap](codeigniter-4-proposed-roadmap.md)! Phase 1 has been completed, and we are embarking on Phase 2.

The first phase has seen the core re-implemented, with a number of essential components:

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

We are now embarking on phase two:

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

The [CodeIgniter4 repository](https://github.com/bcit-ci/CodeIgniter4) has been opened up for community contributions towards milestone 2. We are looking to implement the phase 2 features in a controlled & managed fashion.

We would particularly appreciate additions to the unit testing, with a target code coverage of 80+%.

We are *not* looking for someone's favorite library or package.

We are *not* looking for any shopping cart or authentication packages.

We are looking for contributions that work, with unit testing, user guide changes, changelog entries, and which conform to our style guide. Please read the [Contributing to CodeIgniter](https://bcit-ci.github.io/CodeIgniter4/contributing) section before diving in!

We are using issues, flagged as enhancements or features, to manage the ongoing development of the framework.

We are using issues flagged as bugs for, well, bug tracking.

If you have a feature to propose or discuss, do so on the [CI4 feature request subforum](https://forum.codeigniter.com/forum-29.html). not as github issues.

The [CI4 Support](https://forum.codeigniter.com/forum-30.html) subforum has been created to help community members with setup,environment and process issues.

The [CI4 Discussion](https://forum.codeigniter.com/forum-31.html) has been setup for any other discussions.

We will use this [CI4 Development](https://forum.codeigniter.com/forum-27.html) subforum to report developments to the community.

We are trying to keep the CodeIgniter 4 User Guide in synch with developments. We are shooting for either a nightly build that you have access to, or perhaps a build which is part of a successful travis-ci run. For now, we have setup the new [User Guide](https://bcit-ci.github.io/CodeIgniter4) on github, and it will be updated "frequently".
