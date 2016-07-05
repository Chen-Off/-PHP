phpstorm 组合继承类无法跳转到声明解决 （常规与框架构造模式）
========================================
<b>最近改使用了phpstorm，之前在phpdesigner中可以使用的快捷跳转到声明处功能在phpstorm中使用函数进行加载继承的类无法进行跳转（并且无法自动提示）。</b>
###参考 <a href="http://my.oschina.net/u/248080/blog/351497">开源中国</a> |  <a href="https://github.com/samdark/yii2-cookbook/blob/master/book/ide-autocompletion.md">GITHub链接</a>

<p>之前的例子：</p>
```php
<?php
class one
{
    function one_one()
    {
        echo "123";
    }
}

class two {
    function two_two()
    {
        $this->one = new one();
    }
}

$bb = new two();
$bb->two_two();
$bb->one->one_one();
?>
```

<p>修改后的例子：</p>

```php
<?php
class one
{
    function one_one()
    {
        echo "123";
    }
}

class two {
    function __construct()
    {
        $this->one = '';
    }

    function two_two()
    {
        $this->one = new one();
        return $this->one;
    }
}

$bb = new two();
$one = $bb->two_two();
$one->one_one();
?>
```

上述2处代码都是可以运行的，但只有修改过后的代码可以被快速定位至声明。

原因就是：

1，需要在构造方法中创建可能被加载的类。

2，所加载的类需要进行返回。

3，引用时需要为该类重新声明一个变量或者其他定义。

//注：本人目前不会书写规范的phpdoc 注释代码。好像也可以通道书写规范的注释代码解决该问题。

###使用注释进行设置

1.在该函数的注释返回中返回的是所加载的类的方法名称 `@return`

```php
    /**
     * load_sql
     * @return db
     */
    function load_sql() {
        return new db();
    }
```

`@return` 可以使用 `|` 返回多个方法，如果你的函数是动态加载类的方法的时候

2.在使用函数的类中进行注释定义 `@var`

```php
    class Common
    {
        /**
         * @var db|view|model
         */
        private $db, $view, $model;
    }
```

`@var` 可以使用 `|` 返回多个方法，如果你需要加载多个类方法名称的使用

3.使用 `@property` 设置

4.被调用的函数被赋予了一个方法类，如何使用被赋予的方法类的函数  `@param`

```php
    /**
     * get_data
     * @param $db db
     */
    function get_data($db) {
    }
```

在参数后方加上该方法类的方法名称
