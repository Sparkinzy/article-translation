CodeIgniter 4 HTTP Client

CodeIgniter 4 HTTP 连接

> 原文: [CodeIgniter 4 HTTP Client](http://blog.newmythmedia.com/blog/show/2016-04-18_CodeIgniter_4_HTTP_Client)

It used to be that the majority of the websites were silos, not communicating with other websites much at all. That has changed a lot in the last few years, though, to the point where many sites consume information from external APIs, or communicate with third-party services, like payment gateways, social networks, and more, all on a day-to-day basis. For PHP developers, this typically involves the use of curl to make this happen. That means that any full-stack framework should provide some form of capabilities to help you out there. In the upcoming CodeIgniter 4, we've provided a lightweight, yet still very flexible, HTTP client built-in.

大多数网站都是独立的,与其他网站沟通较少.在最近几年改变很大.但是这些信息都是同过API接口调用.或者与第三方服务交互,类似支付,社交等,每天都有不同的接口.对于php开发者,这些调用通常使用CURL.这意味着开发者在框架中需要一种功能模块来实现,帮助开发(Tips:自己封装也可以).在新版的CodeIgniter 4,我们将提供一个轻量级,且灵活的HTTP内置连接(库).

## The CURLRequest Class

## CURL请求类

The CURLRequest class extends our more generic Request class to provide most of the features that you'd need on a day-to-day basis. It is a synchronous-only HTTP client. Its syntax is modeled very closely after the excellent [Guzzle HTTP Client](http://docs.guzzlephp.org/en/latest/). This way if you find that the built-in client doesn't do everything you need it to, it is very easy to switch over to using Guzzle and take advantage of some of its more advanced features, including asynchronous requests, non-reliance on curl, and more.

这个CURL请求扩展提供满足于每天递增的基础功能的大部分需求.它是一个网络同步的HTTP连接.它语法以很优秀[Guzzle HTTP Client](http://docs.guzzlephp.org/en/latest/)为蓝本
>Tips:Guzzle 是个 PHP 框架，又是个 PHP HTTP 客户端，用来创建 RESTful web 服务客户端。主要特性是通过服务描述快速创建客户端；高效的批量发送大量的请求；持久性连接和并行请求等功能。

当你使用内置的连接就会发现你什么都不需要做,可以很简单的切换到Cuzzle,并利用它的一些先进的功能（异步请求,不依赖于CURL请求等等).

Why build our own solution? For the last decade, many developers have looked to CodeIgniter as the framework that you can download and have 90% or more of the features you need to build your site at your fingertips. Bundling something like Guzzle into the framework doesn't make sense, when Guzzle provides its own HTTP layer, duplicating a large part of the core system code. If we wanted to provide a solution, we had to build our own, based around our own HTTP layer. Being a lightweight solution, it is primarily a wrapper around curl and so the only real trick was ensuring syntax compatibility with Guzzle to make your transitions, if you need to do them, as simple as possible.   

为什么要建立我们自己的解决方案吗？在过去的十年中，许多开发者下载使用CodeIgniter框架,有90%或更多网站功能只需要简单到动动手指就可建立.把Guzzle添加到框架中就显得没有意义的.Guzzle需要提供自己的HTTP层,复制一个大型核心系统的一部分代码.如果我们想提供一个更好的解决方案，必须建立基于自己为主的HTTP层。作为一个轻量级的解决方案，它主要是围绕CURL的一个封装,因此只有真正的诀窍是确保与Guzzle语法兼容，方便您的项目过渡到Guzzle，如果你的语法正确，你会发现非常简单。

> NOTE: This does require that the curl library be installed and usable.

注意：需要CURL库进行安装且使用。

## A Few Quick Examples

##几个简单的例子

Let's walk through a few quick examples to see how easy and convenient it is to have a curl wrapper in our bundle of tricks. These examples are obviously very bare-bones and don't provide all of the details you would need in a finished application.

让我们通过一些简单的例子运行,看我们封装CURL使用有多么容易和方便。这些例子都十分简单，不需要提供一个完成应用程序。

### A Single Call

### 单次请求

Let's say you have another site that you need to communicate with, and that you only need to grab some information from it once, not as part of a larger conversation with the site. You can very simply grab the information you need something like this:

例如你的网站需要访问另一个网站，而且你只需要获取该网站的其中一些信息，而不是发出较大的请求。你可以像这样非常简单抓取到你想要的信息：

```php
$client = new \Config\Services::curlrequest();
$issues = $client->request('get', 'https://api.github.com/repos/bcit-ci/CodeIgniter/issues');
```

The `request()` method takes the HTTP verb and URI and returns an instance of CodeIgniter\HTTP\Response ready for you to use.

这个reques()方法采用HTTP 和 URI 请求，并返回的一个可以使用的实例 CodeIgniter\HTTP\Response。

```php
if ($issues->getStatusCode() == 200)
{
    echo $issues->getBody();
}
```

### Consuming an API

### 多请求接口

If you're working with an API for more than a single call, you can pass the `base_uri` in as one of a number of available options. Then, all following request URIs are appended to that base_uri.

如果你需要是非单一通信接口时，可以通过'base_uri`作为大量请求的选择。把后续所有URI请求被添加到base_uri。

```php
$client = new \Config\Services::curlrequest(['base_uri' => 'https://example.com/api/v1/']);

// GET http://example.com/api/v1/photos
$client->get('photos');

// GET http://example.com/api/v1/photos/13
$client->delete('photos/13');
```

### Submitting A Form

### 提交表单

Often, you will need to submit a form to an external site, sometimes even with file attachments. This is easily handled with the class.

通常情况下，你需要提交一个表单到外部网站，甚至文件附件。这个通过类可以很容易处理。

```php
$client =  new \Config\Servies::curlrequest();
$response = $client->request('POST', 'http://example.com/form', [
    'multipart' => [
        'foo' => 'bar',
        'fuz' => ['bar', 'baz'],
        'userfile' => new CURLFile('/path/to/file.txt')
    ]
]);
```

### Multitude of Options

### 其他功能


The library supports a wide array of options, allowing you to work with nearly any situation you might find yourself up against, including:

这个库支持很多功能，满足你工作中的任何情况，这需要你自己去发现，其中包括：

*   setting auth values for HTTP Basic or Digest authentication
*   setting arbitrary header values for more complex authentication (or any other needs)
*   specifying the location of PEM-formatted certificates,
*   restricting execution time
*   allowing redirects to be followed,
*   return content even when it encounters an error
*   easily upload JSON content
*   send query string or POST variables with the request
*   specify whether SSL certificates should be validated or not (helpful for development servers who don't have full SSL certificates in place)
*   and more.


*   设置基本的HTTP请求或身份认证
*   设置复杂的认证，如的头文件（或者其他需要）
*   指定PEM格式的证书的位置，
*   限制执行时间
*   允许重定向，
*   返回错误报告
*   JSON格式上传
*   发送请求字符串或POST变量
*   检验SSL证书有无（辅助服务商检测SSL证书使用）
*   和更多。

___

Hopefully, this new addition to the framework will come in handy during development and help make using curl much more pleasant.

我们希望，这个新增加的功能在对框架开发过程中派上用场，使curl更简洁。
