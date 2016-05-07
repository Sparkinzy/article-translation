# CodeIgniter 4 的模块

> 原文: [Modules in CodeIgniter 4](http://blog.newmythmedia.com/blog/show/2016-03-15_Modules_in_CodeIgniter_4)

几个月前当我们讨论 CodeIgniter 4 的功能时，HMVC 成为了焦点之一。总结下来，大多数观点把 HMVC 归类成两种用途：一种是显示在页面中的『Widget』，或者是简单的把代码拆分成一个一个的模块。在本篇文章中，我想探讨一下『模块』是如何应用在即将发布的新版框架中的。

> 注意：下面提到的所有例子都是基于预发行版本的，这些内容可能会随时发生变化。

## 模块/HMVC 支持？

在开始前，先让我来澄清一下：CodeIgniter 4 不支持 HMVC 和模块，至少，不是你想象的那种传统的支持方式。在 CodeIgniter 4 中没有一个正式的模块结构的定义，就像你可能在其他框架中看到的一样，诸如 Yii 的扩展或 Drupal 的插件。*并且，在 CodeIgniter 4 中也没有通过不同的目录来完成不同类的优先级加载。*

## 自动加载和命名空间

CodeIgniter 4 自带兼容 PSR-4 的自动加载器，所以不需要 Composer（但随时都可以使用你喜欢的加载器）。

你也许想了解为什么我们不直接使用 Composer 作为框架默认的加载器？在开发团队讨论之初，作为核心开发人员的我曾一度大力支持使用 Composer。然而，通过越来越多的讨论与研究，我们越来越清晰的发现 Composer 对于 CodeIgniter 4 来说，并不是一个正确的选择。其中一个主要的原因是，之前框架的核心部分就曾经使用过一个第三方的脚本，然而每当他改变的时候，框架也必须做出相应的改动来适应他的需求。还有，在不同的服务器环境中，特别是在限制较多的共享主机环境中，使用 Composer 更新时，将非常有可能会产生一系列的问题。最后，因为 Composer 并不是框架的核心部分，所以我们就不必为了支持 Composer 的各种特性而花费大量精力，这使我们可以更专注于框架本身。

在框架中，不仅框架自带的文件支持命名空间，而且你自己的应用程序文件也可以。框架本身自带的文件使用的命名空间是 `CodeIgniter`，application 目录默认使用的是 `App` 命名空间。如果你不想改变默认的命名空间，你可以在控制器和模型中完全使用框架自带的命名空间。使用自定义的命名空间或是框架自带的命名空间都不会影响框架本身的正常工作。

当你结合这两种技术时，支持项目模块的工作，框架已经为你完成了 90%。接着，让我们来看一个简单的实例，然后我们会了解那剩下的 10%。

## 一个简单的实例

设想一下，我们正在创建一个 Blog 模块。要做的第一件事就是确定一个命名空间，然后为所有文件创建一个文件夹。我们会用到公司名称、Standard 和 Blog 这些有意义的子命名空间，因为这些命名空间可以很好的描述这个“模块”。接着我们创建了一个和 `/application` 同级的目录，并且这个目录将会用来存放所有创建的模块。现在，这个文件夹结构可能看起来像你已经使用过的 HMVC：

```
/application
/standard
    /Blog
        /Config
        /Controllers
        /Helpers
        /Libraries
        /Models
        /Views
/system
```

接着打开 `/application/Config/Autoload.php` 文件，这个文件用于让框架知道如何找到那些我们刚刚创建的文件。在这个例子中，我们只在加载器中创建了一个命名空间，尽管你可以为每一个模块创建一个命名空间。

```php
$psr4 = [
        'Config'                     => APPPATH.'Config',
        APP_NAMESPACE.'\Controllers' => APPPATH.'Controllers',
        APP_NAMESPACE                => realpath(APPPATH),
        'Standard'                   => APPPATH.'../standard'
    ];
```

现在，只需要为我们的类创建相对应的命名空间，那么框架就会知道如何去找到这些类，并且在框架的任何地方我们都可以使用他们。

```php
namespace Standard\Blog;

use Standard\Blog\Models\BlogModel;
use Standard\Blog\Libraries\BlogLibrary;
use Standard\Blog\Config\Blog as BlogConfig;

class BlogController extends \CodeIgniter\Controller
{
    public function index()
    {
        $model = new BlogModel();
        $blogLib = new BlogLibrary();
        $config = new BlogConfig();
    }
}
```

真的是非常方便。

## 那些非类文件框架是如何处理的呢？

如果你刚刚仔细看过上面的例子，也许会感到疑惑，“那些没有任何类的文件，就像 Helper 和视图，框架是如何处理的呢？！”正如你所说的，PHP 不能使用命名空间规则加载那些没有任何类的文件。所以，我们增加了一个特性来解决这个问题。

这个特性的工作原理是基于文件的命名空间来定位到文件夹，然后在其子目录中找到它。下面的例子将帮助你理解这个特性。

### 加载 Helper

在本例中，我们假设有一个 `blog_helper` 的文件位于 `/standard/Blog/Helpers/BlogHelper.php`。如果假设这个是类文件，那么他的命名空间看起来就像 `Standard\Blog\Helpers\BlogHelper.php`。我们可以使用框架自带的 `load_helper()` 函数加载：

```php
load_helper('Standard\Blog\Helpers\BlogHelper');
```

哇！就是这么简单。现在你可以找到 Helper 并加载他了。

### 加载视图

视图的加载模式与 Helper 完全相同，可以用 `load_view()` 函数进行加载。

```php
echo load_view('Standard\Blog\Views\index', $data);
```

神奇的是，框架会自动在 CodeIgniter 自带的目录中搜索，所以你不必在路径中包含那些框架自帶的目录名。前面的两个例子也可以这样来写：

```php
load_helper('Standard\Blog\BlogHelper');
echo load_view('Standard\Blog\index', $data);
```


___

然而，这并不是你的项目可使用的唯一文件结构，你完全可以创建属于你自己的结构。我希望你会为框架为你的项目提高了灵活性而感到高兴。
