# Getting Started With CodeIgniter 4 Pre-Alpha 1

> 原文: [Getting Started With CodeIgniter 4 Pre-Alpha 1](http://blog.newmythmedia.com/blog/show/2016-06-25_Getting_Started_With_CodeIgniter_4_Pre-Alpha_1)

Now that the ribbon has been taken off of the first semi-release of CodeIgniter 4, people are wondering how they get started with it. A couple of things have changed that make a big impact on getting started when you're expecting it to be just like CI3. This article aims to help you out there.

## Download It

There are two different ways you can download it currently, but they're both basically the same thing.

1.  From the terminal type: `git clone git@github.com:bcit-ci/CodeIgniter4.git` which will pull down the latest version of the `develop` branch into a new directory called `CodeIgniter4`.
2.  From [GitHub](https://github.com/bcit-ci/CodeIgniter4) do a straight download by clicking `Clone or Download` and then selecting `Download Zip`. Unzip into a new folder when you're done.

## Look Around

If you start taking a look around the new code you'll see a slightly different folder layout than you're used to, though the changes are minimal:

```
/application        - Your app's files go here
/public             - This is the "web root" of your application. index.php is here
/system             - CodeIgniter lives here
/tests              - Holds tests for CodeIgniter and your application
/user_guide_src     - The source files for the user guide. Instructions how to build them.
/writable           - A place for any folder that needs to be writable
```

The two most important right now are the **public** and the **user_guide_src** folders. **public** holds the new web root of the application. Best practices tell us that your application should live outside of the web and not be accessible from a browser. With this layout, only the files in **public** are available to end-users through the browser. This is in line with every other major framework out there, but is a change from the way that CodeIgniter has always done it.

The **user_guide_src** folder contains all of the current documentation for the framework. To the best of our knowledge it is completely up to date with the current release, and we plan on keeping it in sync as we go. This will be your best friend, as you explore over the coming days or weeks. While this isn't the generated HTML, all of the files in the **source** folder inside it are human-readable and laid out similarly to what you're used to in CI3 docs. Take time to read through the new things in here as most of your questions should be answered, and you'll hopefully find some nice surprises lurking in places.

The following pages are good reads to get started with:

*   [Application Structure](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/concepts/structure.rst)
*   [Autoloading Files](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/concepts/autoloader.rst)
*   [Services](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/concepts/services.rst)
*   [Global Functions and Constants](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/general/common_functions.rst)
*   [URI Routing](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/general/routing.rst)
*   [Controllers](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/general/controllers.rst)
*   [Models](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/database/model.rst)
*   [Views](https://github.com/bcit-ci/CodeIgniter4/blob/develop/user_guide_src/source/general/views.rst)

## Start Playing

In order to start playing around with the new code, you'll need to get it running in a browser. There's a number of ways to do it, but we'll cover two here.

### PHP Server

PHP has a built-in web server now. This is the simplest way to get running. Jump to the command line, navigate to the public directory, and type the following:

```
$ php -S localhost:8000
```

That's it. Back to your browser, navigate to **http://localhost:8000** and you should see the shiny new CI4 welcome page.

### Virtual Host

A more permenant solution is to have another web server running locally, like Apache or NGINX, and create a new virtual host for it. This allows you to create a name for the site, like **ci4.dev**, and use that in your browser. This is especially helpful so that you don't have to worry about RewriteBase commands for Apache config, or any other tricky ways to get past the **public** folder. When you setup the virtual host, make sure it is pointing to the **public** folder or it won't work.

Here are some helper guides for those of you using popular AMP stacks locally:

*   [MAMP](http://foundationphp.com/tutorials/vhosts_mamp.php)
*   [XAMPP](http://foundationphp.com/tutorials/apache_vhosts.php)
*   [WAMP](http://www.techrepublic.com/blog/smb-technologist/create-virtual-hosts-in-a-wamp-server/)

Note that most of these are essentially the same thing, since you're editing raw Apache config files.

[Laravel Homestead](https://laravel.com/docs/5.2/homestead) is another excellent solution for a PHP7 virtual machine running under NGINX.

----

Lastly, have fun!
