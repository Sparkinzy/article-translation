# Using Entities in CodeIgniter 4

> 原文: [Using Entities in CodeIgniter 4](http://blog.newmythmedia.com/blog/show/2016-10-25_Entities_In_CodeIgniter_4)

The [Repository Design Pattern](https://code.tutsplus.com/tutorials/the-repository-design-pattern--net-35804) is very useful in application design. At the core of this, though, is the Entity, a class that simply represents a single object of whatever data you are modeling. This could be a single User, a Comment, an Article, or anything else in your app. The trick is to ensure that’s all it is. It can (and should!) handle some of the business logic, and can include convenience methods to combine data in various ways, or work with other Repositories to get related data. But it shouldn’t have a care in the world about how to persist itself. That’s the Repository’s job.

In today’s tutorial, we’ll skip all of the steps of using a Repository and just look at how to make working with Entity classes as simple as possible, while still being very flexible.

## Getting Entities from the Model

The first thing to look at is how do we get this data from the model itself? Lucky for us, CodeIgniter’s [Database layer](https://bcit-ci.github.io/CodeIgniter4/database/results.html#custom-result-objects) has been able to convert the results from database queries into custom classes since at least version 2. I didn’t know about this until version 3 was about ready to be released, as it wasn’t documented at the time. Doing this is as easy as passing fully-qualified class name as the only parameter to the `getResult()` method:

```php
$rows = $db->table(‘users’)->limit(20)->get();
$user = $rows->getResult(‘App\Entities\User’);
```

This would give you an array of User instances. As long as the Entity class has properties that a) match the names of the columns in the database, and b) the model can access those parameters, their values will be filled in and you’ll be able to work with the classes straight-away. Let’s take a look at some very basic versions to see this in action.

The `UserModel` might look something like this:

```php
<?php namespace App\Models;

use CodeIgniter\Model;

class UserModel extends Model
{
    protected $table = ‘users’;
    protected $returnType = ‘App\Entities\User’;
    protected $useSoftDeletes = false;
    protected $allowedFields = [
        ‘username’, ‘email’, ‘password_hash’
    ];
    protected $useTimestamps = true;
}
```

Notice the `$returnType` property? By setting this to the Entity class, all of our results will be returned as instances of `App\Entities\User` by default. We can always change that at runtime if we need to.

Also, notice the `$allowedFields` property. This is especially important for a couple of reasons. The first is that the Model class forces you to fill this in if you want to pass it an array of key/value pairs during a `save()`, `insert()`, or `update()` call to help protect against Mass Assignment attacks, or simple human error. But it will also come in handy when we want to save the object back to the database. More on that a little later.

### The Entity Class

Now lets look at the simplest version of the Entity class that we can make. In the next blog post we’ll explore a much more powerful version. We would create a new file at **/application/Entities/User.php**, creating the new Entities folder since it’s not there by default. It might look something like this:

```php
<?php namespace App\Entities;

class User 
{
    public $id;
    public $username;
    public $email;
    public $password_hash;
    public $created_at;
    public $updated_at;

    public function setPassword(string $password)
    {
        $this->password_hash = password_hash($password, PASSWORD_DEFAULT);

        return $this;
    }

    public function createdOn($format = ‘Y-m-d’)
    {
        $date = new DateTime($this->created_at);
        return $date->format($format);
    }
}
```

As I said, this is an extremely basic version. But it works to demonstrate the usefulness. It contains properties for all of the columns in the database, not just the ones listed in `$allowedFields`. Since they are all public, they can be accessed from outside the class whenever you need them, whether in a controller, view, or in the model itself when saving. In real apps, we would likely want to make those protected properties to keep things a little safer. We’ll look at that in the next post, and combine it with some powerful convenience methods to really make working with Entities simple and, dare I say it, fun.

This small example, also includes two convenience methods. The first helps ensure a business rule by make a single place that determines how our password is hashed. The second allows you to retrieve a formatted version of the created_at date. Neither of these are ground-breaking. They’re only there to give you some ideas of basic methods you might find helpful.

## Saving the Entity

CodeIgniter’s Model class is smart enough to recognize Entity classes whenever you perform a `save()`, `update()`, or `insert()` call, and convert that class to an array of key/value pairs that can be used to create or update the record in the database.

But, how does it know which fields should be allowed to update the database? Remember the `$allowedFields` array the model has? That’s the key. It uses that list of fields and grabs the values from the Entity class. In our example, it would create an array that looks something like:

```php
[
    ‘username’ => ‘blackjack’,
    ‘email’ => ‘jack.black@example.com’,
    ‘password_hash’ => ‘. . .’
]
```

Notice that it did not grab the `id`, `created_at`, or `update_at` fields. That’s because the `id` field is automatically assigned by the database and we shouldn’t be able to change it, and the date fields are managed by the Model class itself, and we don’t want outside classes mucking with the dates.

So, when it comes to saving your data, there’s nothing special to do. Just pass your Entity to the `save()`, `update()`, or `insert()` method, and the Model takes care of the rest.

```php
// Grab a user
$user = $userModel->find(15);

// Update their email
$user->email = ‘blackjack@example.com’;

// Save the user
$userModel->save($user);
```

## Up Next

Hopefully, this gets you interested in exploring this type of pattern with your applications. Even if you don’t do a full-repository pattern, this simple change makes your code much more manageable and can be very powerful.

In the next post, we’ll take it a little farther by hiding those class properties, but still ensuring they’re accessible to the Model during creation and saving. Then we’ll craft a small Entity class that we can base all of our Entities from that provides some magic that allows us to manipulate the data on the way in and out, and even provide a new `fill()` method that takes an array and populates/changes the class properties. All of this allows for much more freedom, power, and flexibility in your Entity classes.
