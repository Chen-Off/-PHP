phpstorm 组合继承类无法跳转到声明解决 （常规与框架构造模式）
========================================
<b>最近改使用了phpstorm，之前在phpdesigner中可以使用的快捷跳转到声明处功能在phpstorm中使用函数进行加载继承的类无法进行跳转。</b>

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
      
