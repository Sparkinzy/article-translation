# Better Entities in CodeIgniter 4

> 原文: [Better Entities in CodeIgniter 4](http://blog.newmythmedia.com/blog/show/2016-10-27_Better_Entities_In_CodeIgniter_4)

Continuing on from the [previous post](entities-in-codeigniter-4.md), this tutorial will look at taking our basic Entities and making the more flexible, and more capable. Again, this isn’t meant to be a full demonstration of the Repository pattern, but simply examining one particular aspect of it.

What was wrong with the previous example? For starters, all of our class properties had to be public to allow it to work with the Model. While that’s not the worst thing in the world, it is definitely not ideal.

## Getters and Setters

The first step is to make the class properties `protected` instead of `public`. In order to make those visible to the Model, we’ll use the `__get()` and `__set()` magic methods to provide access.

```php
public function __get(string $key)
{
    if (isset($this->$key))
    {
        return $this->$key;
    }
}

public function __set(string $key, $value = null)
{
    if (isset($this->$key))         
    { 
        $this->$key = $value;       
    }
}
```

This solves our problem, but simply adds extra code between the value and your code, which is good for encapsulation, but we can do better here. There are going to be numerous times that you want to perform some logic whenever we a value on the Entity. For example, you might want to automatically hash a password whenever it’s set. Or you might want to always keep your dates stored as DateTime instances. So, how do we make this simple?

For that, let’s add some new functionality to the setter that allows us to call any method that had `set_` and then property name, like `set_password`.

```php
public function __set(string $key, $value = null)
{
    // if a set* method exists for this key,
    // use that method to insert this value.
    $method = 'set_'.$key;
    if (method_exists($this, $method))
    {
        $this->$method($value);
    }
    // A simple insert on existing keys otherwise.
    elseif (isset($this->$key))
    {
        $this->$key = $value;
    }
}
```

Now, you could solve your business needs with simple little functions like these:

```php
protected function set_password(string $value)
{
    $this->password = password_hash($value, PASSWORD_BCRYPT);
}

protected function set_last_login(string $value)
{
    $this->last_login = new DateTime($value);
}
```

Whenever you set **$user->password = ‘abc’**, or **$user->last_login = ’10-15-2016 3:42pm’** your custom methods will automatically be called, storing the property as your business needs dictate. Let’s do the same thing for the getters.

```php
public function __get(string $key)
{
    // if a set* method exists for this key,
    // use that method to insert this value.
    if (method_exists($this, $key))
    {
        return $this->$key();
    }
    if (isset($this->$key))
    {
        return $this->$key;
    }
}
```

In this case, we’re simply checking for a method with the exact name as the class property. You can set these methods as public and then it would work the same, no matter whether it was called as a method or a property, **$user->last_login** or **$user->last_login()**:

```php
public function last_login($format=‘Y-m-d H:i:s’)
{
    return $this->last_login->format($format);
}
```

By setting a default value for **$format**, it works either way, but you can now get the value in the format you need it at that time, instead of being restricted to a single format.

## Filler

This has already helped our classes to become more capable and flexible, at the same time helping us to maintain our business rules, and still easily be saved to the database and gotten back out again intact. But wouldn’t it be nice if we could just shove an array of key/value pairs at the class and have it fill the properties out automatically, but only work with existing properties? This makes it simple to grab data from $_POST, create a new Entity class, and shove it there before saving. Even better if we can customize data on the way in the same way we did with setters, right? Welcome to the `fill()` method:

```php
public function fill(array $data)
{
    foreach ($data as $key => $var)
    {
        $method = 'fill_'. $key;
        if (method_exists($this, $method))
        {
            $this->$method($var);
        }
        elseif (property_exists($this, $key))
        {
            $this->$key = $var;
        }
    }
}
```

A quick example should make this one make sense. First, let’s grab the POST data, add it to a new User object, and save it to the database:

```php
$data = $_POST;
$user = new App\Entities\User();
$user->fill($data);
$userModel->save($user);
```

If this were a registration form we were handling, we might be getting a `password` field that we wanted to make sure was hashed. So, a quick `fill_` method and we’re good to go. For this example, we’ll just re-use the setter we created earlier:

```php
protected function fill_password(string $value)_
{
    $this->set_password($value);
}
```

## The Entity Class

To make this all simple to re-use, we should create a new `Entity` class that our Entities can extend and get these features automatically. Here’s one such class, that also takes care of our timestamps, including timezone conversions.

```php
<?php namespace Myth;

/**
 * Class Entity
 *
 * A base class for entities that provides some convenience methods
 * that make working with entities a little simpler.
 *
 * @package App\Entities
 */
class Entity
{
    /**
     * Given an array of key/value pairs, will fill in the
     * data on this instance with those values. Only works
     * on fields that exist.
     *
     * @param array $data
     */
    public function fill(array $data)
    {
        foreach ($data as $key => $var)
        {
            $method = 'fill_'. $key;
            if (method_exists($this, $method))
            {
                $this->$method($var);
            }

            elseif (property_exists($this, $key))
            {
                $this->$key = $var;
            }
        }
    }

    //--------------------------------------------------------------------

    //--------------------------------------------------------------------
    // Getters
    //--------------------------------------------------------------------

    /**
     * Returns the created_at field value as a string. If $format is
     * a string it will be used to format the result by. If $format
     * is TRUE, the underlying DateTime option will be returned instead.
     *
     * Either way, the timezone is set to the value of $this->timezone,
     * if set, or to the app's default timezone.
     *
     * @param string $format
     *
     * @return string
     */
    public function created_at($format = 'Y-m-d H:i:s'): string
    {
        $timezone = isset($this->timezone)
            ? $this->timezone
            : app_timezone();

        $this->created_at->setTimezone($timezone);

        return $format === true
            ? $this->created_at
            : $this->created_at->format($format);
    }

    //--------------------------------------------------------------------

    /**
     * Returns the updated_at field value as a string. If $format is
     * a string it will be used to format the result by. If $format
     * is TRUE, the underlying DateTime option will be returned instead.
     *
     * Either way, the timezone is set to the value of $this->timezone,
     * if set, or to the app's default timezone.
     *
     * @param string $format
     *
     * @return string
     */
    public function updated_at($format = 'Y-m-d H:i:s'): string
    {
        $timezone = isset($this->timezone)
            ? $this->timezone
            : app_timezone();

        $this->updated_at->setTimezone($timezone);

        return $format === true
            ? $this->updated_at
            : $this->updated_at->format($format);
    }

    //--------------------------------------------------------------------

    //--------------------------------------------------------------------
    // Setters
    //--------------------------------------------------------------------

    /**
     * Custom value for the `created_at` field used with timestamps.
     *
     * @param string $datetime
     *
     * @return $this
     */
    public function set_created_at(string $datetime)
    {
        $this->created_at = new \DateTime($datetime, new \DateTimeZone('UTC'));

        return $this;
    }

    //--------------------------------------------------------------------

    /**
     * Custom value for the `updated_at` field used with timestamps.
     *
     * @param string $datetime
     *
     * @return $this
     */
    public function set_updated_at(string $datetime)
    {
        $this->updated_at = new \DateTime($datetime, new \DateTimeZone('UTC'));

        return $this;
    }

    //--------------------------------------------------------------------

    //--------------------------------------------------------------------
    // Magic
    //--------------------------------------------------------------------

    /**
     * Allows Models to be able to get any class properties that are
     * stored on this class.
     *
     * For flexibility, child classes can create `get*()` methods
     * that will be used in place of getting the value directly.
     * For example, a `created_at` property would have a `created_at`
     * method.
     *
     * @param string $key
     *
     * @return mixed
     */
    public function __get(string $key)
    {
        // if a set* method exists for this key,
        // use that method to insert this value.
        if (method_exists($this, $key))
        {
            return $this->$key();
        }

        if (isset($this->$key))
        {
            return $this->$key;
        }
    }

    //--------------------------------------------------------------------

    /**
     * Allows Models to be able to set any class properties
     * from the result set.
     *
     * For flexibility, child classes can create `set*()` methods
     * to provide custom setters for keys. For example, a field
     * named `created_at` would have a `set_created_at` method.
     *
     * @param string $key
     * @param null   $value
     */
    public function __set(string $key, $value = null)
    {
        // if a set* method exists for this key,
        // use that method to insert this value.
        $method = 'set_'.$key;
        if (method_exists($this, $method))
        {
            $this->$method($value);
        }

        // A simple insert on existing keys otherwise.
        elseif (isset($this->$key))
        {
            $this->$key = $value;
        }
    }

    //--------------------------------------------------------------------
}
```
