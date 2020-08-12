# 环境配置

在windows上的环境配置：可以下一个Wampserver，启动即可。注意这个软件默认是必须下载mariadb的，而且占用了3306端口，因此mysql的端口默认是3308。IDE可以使用PhpStorm，php解释器设置为wamp的安装目录下的bin/php/phpx.x.xx/php.exe.（x代表php版本号）

# 基础



## PHP语法

```php+HTML
<!DOCTYPE html> 
<html> 
<body> 

<h1>My first PHP page</h1> 

<?php 
echo "Hello World!"; 
?> 

</body> 
</html>
```

注释与java类似。

print  echo 为输出。

## 注意

1. **方法名，函数名，类名，关键字，不区分大小写。**变量名区分大小写。

2. 数组不只可以用array()函数来定义，也可以直接使用$arr = [1,2,3];  和Python一样。

3. echo一个双引号包裹的东西，里面的变量名会解析为变量而不是字符串，但如果用单引号就会解析为字符串。

4. 如果一个数组内既有关联元素，也有下标元素，那么只有下标元素可以通过下标访问，且下标按序从0递增，不受关联元素影响。但是关联元素也会占用一个数组的空间（数组长度+1）。

5. ```php
   $a = array(
       null => 'a',
       true => 'b',
       false => 'c',
       0 => 'd',
       1 => 'e',
       '' => 'f'
   );
   //这个代码中$a的长度为3, 因为键名将被这样转换：null 转为(空字符串)，true 转为 1，false 转为 0。这样最后加入的d，e，f会替换前三个元素。
   ```

6. 数组中的下标元素也可看做自带默认键（数字类型，即元素下标）的关联元素。$arr[0] 这种实际上就是找到键为0 的value。

## 变量

```php
<?php
$x=5;
$y=6;
$z=$x+$y;
echo $z;
?>
```

规则：

- 变量以 $ 符号开始，后面跟着变量的名称
- 变量名必须以字母或者下划线字符开始
- 变量名只能包含字母数字字符以及下划线（A-z、0-9 和 _ ）
- 变量名不能包含空格
- 变量名是区分大小写的（$y 和 $Y 是两个不同的变量）

### 作用域

- local

  - 与Python类似，函数内部为局部变量，外部为全局变量

- global

  - 全局变量在函数内访问，需要global声明

  - ```php
    <?php
    $x=5;
    $y=10;
     
    function myTest()
    {
        global $x,$y;
        $y=$x+$y;
    }
     
    myTest();
    echo $y; // 输出 15
    ?>
    ```

  - PHP 将所有全局变量存储在一个名为 $GLOBALS[*index*] 的数组中。  *index* 保存变量的名称。这个数组可以在函数内部访问，也可以直接用来更新全局变量。

    ```php
    <?php
    $x=5;
    $y=10;
     
    function myTest()
    {
        $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y'];
    } 
     
    myTest();
    echo $y;
    ?>
    ```

    

- static

  - 函数内声明的变量在函数结束时就删除，如果想保留，比如每次调用函数时某个变量的值都保存记录，就是要static修饰该变量。

    ```php
    <?php
    function myTest()
    {
        static $x=0;
        echo $x;
        $x++;
        echo PHP_EOL;    // 换行符
    }
     
    myTest();
    myTest();
    myTest();
    ?>
    ```

    

- parameter

  - 与Python的参数传递一致，注意参数名为：$xxx

## echo和print

echo 和 print 区别:

- echo - 可以输出一个或多个字符串
- print - 只允许输出一个字符串，返回值总为 1

**提示：**echo 输出的速度比 print 快， echo 没有返回值，print有返回值1。

```php
<?php
echo "<h2>PHP 很有趣!</h2>";
echo "Hello world!<br>";
echo "我要学 PHP!<br>";
echo "这是一个", "字符串，", "使用了", "多个", "参数。";
?>
    
<?php
$txt1="学习 PHP";
$txt2="RUNOOB.COM";
$cars=array("Volvo","BMW","Toyota");
 
echo $txt1;
echo "<br>";
echo "在 $txt2 学习 PHP ";
echo "<br>";
echo "我车的品牌是 {$cars[0]}";
?>
```

```php
<?php
$txt1="学习 PHP";
$txt2="RUNOOB.COM";
$cars=array("Volvo","BMW","Toyota");
 
print $txt1;
print "<br>";
print "在 $txt2 学习 PHP ";
print "<br>";
print "我车的品牌是 {$cars[0]}";
?>
```

echo和print都可以使用或去掉括号。

## 数据类型

- 整型
- 字符串
  - 单引号或双引号
- 浮点
- 布尔
- 数组
- 对象
- NULL

```php
<?php
$x = 10.23;
$x = "1123";
$x = true;
$x = 1;
//数组
$x = array("a","s");
var_dump($x);//输出数据类型和值
$x = null;
//对象
class Car
{
  var $color;
  function __construct($color="green") {
    $this->color = $color;
  }
  function what_color() {
    return $this->color;
  }
}
?>
```

## 类型比较

- 松散比较：使用两个等号 **==** 比较，只比较值，不比较类型。
- 严格比较：用三个等号 **===** 比较，除了比较值，也比较类型。

0，false，null，“”的值相同，类型不同。

“0”和false，0，的值相同，“0”和null，“”，的值不相同。

python中 is比较地址，==比较内容。

## 常量

设置常量使用define（）函数，语法如下：

```php
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
```

```php

// 区分大小写的常量名
define("GREETING", "欢迎访问 Runoob.com");
echo GREETING;    // 输出 "欢迎访问 Runoob.com"
echo '<br>';
echo greeting;   // 输出 "greeting"

```



常量默认是全局的，函数外定义后，函数内访问无需global关键字。

## 字符串

### 唯一的字符串运算符

并置运算符，用于连接字符串。相当于python中的+

```php
<?php 
$txt1="Hello world!"; 
$txt2="What a nice day!"; 
echo $txt1 . " " . $txt2; 
?>
```

### strlen()函数

返回字符串长度（字节数，英文符号占一个字节，汉字三个字节）

数组长度用count函数。

### strpos()函数

找匹配

```php
<?php 
echo strpos("Hello world!","world"); 
?>
```

## 运算符

### 加减乘除

与python一致，整除不是用 // ，而是用PHP7+版本运算符intdiv(1,3) = 3。

### 递加递减

++x;  x++; --x; x--;与Java类似

### 比较

- == 值等于
- === 类型和值都等于
- ！==绝对不等于（值或类型有一个不等）

### 逻辑

- and  &&
- or   ||
- xor 异或
- !x 非

### 数组运算符



| 运算符  | 名称   | 描述                                                         |
| ------- | ------ | ------------------------------------------------------------ |
| x + y   | 集合   | x 和 y 的集合                                                |
| x == y  | 相等   | 如果 x 和 y 具有相同的键/值对，则返回 true                   |
| x === y | 恒等   | 如果 x 和 y 具有相同的键/值对，且顺序相同类型相同，则返回 true |
| x != y  | 不相等 | 如果 x 不等于 y，则返回 true                                 |
| x <> y  | 不相等 | 如果 x 不等于 y，则返回 true                                 |
| x !== y | 不恒等 | 如果 x 不等于 y，则返回 true                                 |

### 组合比较符(PHP7+)

PHP7+ 支持组合比较符（combined comparison operator）也称之为太空船操作符，符号为 **<=>**。组合比较运算符可以轻松实现两个变量的比较，当然不仅限于数值类数据的比较。

语法格式如下：

```php
$c = $a <=> $b;
```

解析如下：

- 如果 **$a > $b**, 则 **$c** 的值为 **1**。
- 如果 **$a == $b**, 则 **$c** 的值为 **0**。
- 如果 **$a < $b**, 则 **$c** 的值为 **-1**。

## if语句

```php
if (条件)
{
    if 条件成立时执行的代码;
}
elseif (条件)
{
    elseif 条件成立时执行的代码;
}
else
{
    条件不成立时执行的代码;
}
```

## switch

```php
<?php
$favcolor="red";
switch ($favcolor)
{
case "red":
    echo "你喜欢的颜色是红色!";
    break;
case "blue":
    echo "你喜欢的颜色是蓝色!";
    break;
case "green":
    echo "你喜欢的颜色是绿色!";
    break;
default:
    echo "你喜欢的颜色不是 红, 蓝, 或绿色!";
}
?>
```

## 数组/字典

数值数组：带有数字id键

```php
<?php
$cars=array("Volvo","BMW","Toyota");
$arrlength=count($cars);
 
for($x=0;$x<$arrlength;$x++)
{
    echo $cars[$x];
    echo "<br>";
}
?>
```

关联数组：相当于字典

```php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
//或者这样定义：
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43"; 

// 输出
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";

```

### 输出数组

printr（$arr）;

### 遍历关联数组：

```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
foreach($age as $x=>$x_value)
{
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
?>
```

## 数组排序

- sort() - 对数组进行升序排列
- rsort() - 对数组进行降序排列
- asort() - 根据关联数组的值，对数组进行升序排列
- ksort() - 根据关联数组的键，对数组进行升序排列
- arsort() - 根据关联数组的值，对数组进行降序排列
- krsort() - 根据关联数组的键，对数组进行降序排列

```php
$arr = array("a","c","b");
sort($arr);
```

## 超级全局变量

- $GLOBALS

  PHP 将所有全局变量存储在一个名为 $GLOBALS[*index*] 的关联数组（字典）中。  *index* 保存变量的名称。这个数组可以在函数内部访问，也可以直接用来更新全局变量。

- $_SERVER

  $_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。不能保证每个服务器都提供全部项目；服务器可能会忽略一些，或者提供一些没有在这里列举出来的项目。 

  | 元素/代码                | 描述                                                         |
  | :----------------------- | :----------------------------------------------------------- |
  | $_SERVER['SCRIPT_NAME']  | 包含当前脚本的路径。这在页面需要指向自己时非常有用。__FILE__ 常量包含当前脚本(例如包含文件)的完整路径和文件名。 |
  | $_SERVER['PHP_SELF']     | 当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://example.com/test.php/foo.bar 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar。__FILE__ 常量包含当前(例如包含)文件的完整路径和文件名。 从 PHP 4.3.0 版本开始，如果 PHP 以命令行模式运行，这个变量将包含脚本名。之前的版本该变量不可用。 |
  | $_SERVER['SERVER_NAME']  | 当前运行脚本所在的服务器的主机名。如果脚本运行于虚拟主机中，该名称是由那个虚拟主机所设置的值决定。(如: www.runoob.com) |
  | $_SERVER['HTTP_HOST']    | 当前请求头中 Host: 项的内容，如果存在的话。                  |
  | $_SERVER['HTTP_REFERER'] | 引导用户代理到当前页的前一页的地址（如果存在）。由 user agent 设置决定。并不是所有的用户代理都会设置该项，有的还提供了修改 HTTP_REFERER 的功能。简言之，该值并不可信。) |

- $_REQUEST

  PHP $_REQUEST 用于收集HTML表单提交的数据。

  以下实例显示了一个输入字段（input）及提交按钮(submit)的表单(form)。 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至<form>标签中 action 属性中指定的脚本文件。 在这个实例中，我们指定文件来处理表单数据。如果你希望其他的PHP文件来处理该数据，你可以修改该指定的脚本文件名。 然后，我们可以使用超级全局变量 $_REQUEST 来收集表单中的 input 字段数据:

  ```php+HTML
  <html>
  <body>
   
  <form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
  </form>
   
  <?php 
  $name = $_REQUEST['fname']; 
  echo $name; 
  ?>
   
  </body>
  ```

  

- $_POST

  收集表单数据，在HTML form标签的指定该属性："method="post"。

- $_GET

  应用于收集表单数据，在HTML form标签的指定该属性："method="get"。

  $_GET 也可以收集URL中发送的数据。

- $_FILES

- $_ENV

- $_COOKIE

- $_SESSION

## 循环

- while 

- do while

- for

以上都与Java类似

- foreach循环：

```php
<?php
$x=array("one","two","three");
foreach ($x as $value)
{
    echo $value . "<br>";
}
?>
```

## 函数

```php
<?php
function add($x,$y)
{
    $total=$x+$y;
    return $total;
}
 
echo "1 + 16 = " . add(1,16);
?>
```

JS+Python的结合。

## 魔术常量

有八个魔术常量，它们的值随着它们在代码中的位置改变而改变。

- ```php
  __LINE__  当前行号
  __FILE__   文件的完整路径和文件名，绝对路径
  __DIR__    文件所在目录
  __FUNCTION__   __CLASS__  __METHOD__(5.0.0新加) 所在函数名和所在类名和所在方法名
  __NAMESPACE__  当前命名空间名
      
  ```

## 命名空间

PHP 命名空间可以解决以下两类问题：

1. 用户编写的代码与PHP内部的类/函数/常量或第三方类/函数/常量之间的名字冲突。
2. 为很长的标识符名称(通常是为了缓解第一类问题而定义的)创建一个别名（或简短）的名称，提高源代码的可读性。

将全局的非命名空间中的代码与命名空间中的代码组合在一起，只能使用大括号形式的语法。全局代码必须用一个不带名称的 namespace 语句加上大括号括起来，例如：

```php
<?php
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```

**注意：**在声明命名空间之前唯一合法的代码是用于定义源文件编码方式的 declare 语句。所有非 PHP 代码包括空白符都不能出现在命名空间的声明之前。

```php
<?php
declare(encoding='UTF-8'); //定义多个命名空间和不包含在命名空间中的代码
namespace MyProject {}
?>
```



```php+HTML
<html> 
<?php namespace MyProject; // 命名空间前出现了“<html>" 会致命错误 -　命名空间必须是程序脚本的第一条语句 ?>
```



## 面向对象

```php
<?php
class Site {
  /* 成员变量 */
  var $url;
  var $title;
  
  /* 成员函数 */
  function setUrl($par){
     $this->url = $par;
  }
  
  function getUrl(){
     echo $this->url . PHP_EOL;
  }
  
  function setTitle($par){
     $this->title = $par;
  }
  
  function getTitle(){
     echo $this->title . PHP_EOL;
  }
}
?>
```

变量 **$this** 代表自身的对象。

**PHP_EOL** 为换行符,相当于“/n”。

### 创建对象

```php
$runoob = new Site;
$taobao = new Site;
$google = new Site;
// 调用函数
// 调用成员函数，设置标题和URL
$runoob->setTitle( "菜鸟教程" );
$taobao->setTitle( "淘宝" );
$google->setTitle( "Google 搜索" );

$runoob->setUrl( 'www.runoob.com' );
$taobao->setUrl( 'www.taobao.com' );
$google->setUrl( 'www.google.com' );

// 调用成员函数，获取标题和URL
$runoob->getTitle();
$taobao->getTitle();
$google->getTitle();

$runoob->getUrl();
$taobao->getUrl();
$google->getUrl();
```

### 构造函数

主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，在创建对象的语句中与 **new** 运算符一起使用。

在上面的例子中我们就可以通过构造方法来初始化 $url 和 $title 变量：



```php
function __construct( $par1, $par2 ) {
   $this->url = $par1;
   $this->title = $par2;
}
//这样实例化时就不需要直接设置初始值，相当于python中的init函数
$runoob = new Site('www.runoob.com', '菜鸟教程'); 
$taobao = new Site('www.taobao.com', '淘宝'); 
```

### 析构函数

析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。

```php
<?php
class MyDestructableClass {
   function __construct() {
       print "构造函数\n";
       $this->name = "MyDestructableClass";
   }

   function __destruct() {
       print "销毁 " . $this->name . "\n";
   }
}

$obj = new MyDestructableClass();
?>
```

### 支持单继承

```php
class Child extends Parent {
   // 代码部分
}
```

### 重写

继承的方法可以重写，与重新定义新方法没什么区别。

### 访问控制

PHP 对属性或方法的访问控制，是通过在前面添加关键字 public（公有），protected（受保护）或 private（私有）来实现的。

- **public（公有）：**公有的类成员可以在任何地方被访问。
- **protected（受保护）：**受保护的类成员只能被其自身以及其子类和父类（中通过代码）访问。（无法在全局代码中访问）
- **private（私有）：**私有的类成员则只能被其定义所在的类访问。

类属性必须定义为公有，受保护，私有之一。如果用 var 定义，则被视为公有。没有任何关键字则默认public。

### 接口

与Java类似，接口中只有方法的声明，一个类可实现多个接口。

```php
<?php

// 声明一个'iTemplate'接口
interface iTemplate
{
    public function setVariable($name, $var);
    public function getHtml($template);
}


// 实现接口
class Template implements iTemplate
{
    private $vars = array();
  
    public function setVariable($name, $var)
    {
        $this->vars[$name] = $var;
    }
  
    public function getHtml($template)
    {
        foreach($this->vars as $name => $value) {
            $template = str_replace('{' . $name . '}', $value, $template);
        }
 
        return $template;
    }
}
```

### 常量

类中保持不变的量，用const关键字，不需要$符号（与函数类似）。

```php
<?php
class MyClass
{
    const constant = '常量值';

    function showConstant() {
        echo  self::constant . PHP_EOL;
    }
}

echo MyClass::constant . PHP_EOL;

$classname = "MyClass";
echo $classname::constant . PHP_EOL; // 自 5.3.0 起

$class = new MyClass();
$class->showConstant();

echo $class::constant . PHP_EOL; // 自 PHP 5.3.0 起
?>
```

类中const修饰的常量相当于static的，可以通过 类名或实例名::来访问，static的方法同样也是。

### 抽象类

与Java类似。至少一个函数是抽象的，无法实例化。

```php
<?php
abstract class AbstractClass
{
 // 强制要求子类定义这些方法
    abstract protected function getValue();
    abstract protected function prefixValue($prefix);

    // 普通方法（非抽象方法）
    public function printOut() {
        print $this->getValue() . PHP_EOL;
    }
}

class ConcreteClass1 extends AbstractClass
{
    protected function getValue() {
        return "ConcreteClass1";
    }

    public function prefixValue($prefix) {
        return "{$prefix}ConcreteClass1";
    }
}

class ConcreteClass2 extends AbstractClass
{
    public function getValue() {
        return "ConcreteClass2";
    }

    public function prefixValue($prefix) {
        return "{$prefix}ConcreteClass2";
    }
}

$class1 = new ConcreteClass1;
$class1->printOut();
echo $class1->prefixValue('FOO_') . PHP_EOL;

$class2 = new ConcreteClass2;
$class2->printOut();
echo $class2->prefixValue('FOO_') . PHP_EOL;
?>
```

**注意：**子类方法可以包含父类抽象方法中不存在的可选参数。

```php
<?php
abstract class AbstractClass
{
    // 我们的抽象方法仅需要定义需要的参数
    abstract protected function prefixName($name);

}

class ConcreteClass extends AbstractClass
{

    // 我们的子类可以定义父类签名中不存在的可选参数
    public function prefixName($name, $separator = ".") {
        if ($name == "Pacman") {
            $prefix = "Mr";
        } elseif ($name == "Pacwoman") {
            $prefix = "Mrs";
        } else {
            $prefix = "";
        }
        return "{$prefix}{$separator} {$name}";
    }
}
```

### static关键字

与Java类似，

- 可以不实例化类而直接访问。

- 由于静态方法不需要通过对象即可调用，所以伪变量 $this 在静态方法中不可用。

- **静态属性不可以由对象通过 -> 操作符来访问，但静态方法可以。**

```php
<?php
class Foo {
  public static $my_static = 'foo';
  
  public function staticValue() {
     return self::$my_static;
  }
}

print Foo::$my_static . PHP_EOL;
$foo = new Foo();

print $foo->staticValue() . PHP_EOL;
?>    
```

### self和$this

**1.self代表类，$this代表对象**
**2.能用$this的地方一定使用self，能用self的地方不一定能用$this**
**静态的方法中不能使用$this，静态方法给类访问的。**

### final关键字

被修饰的

- 类无法继承

- 方法无法重写

### 调用父类构造方法

```php
<?php
class BaseClass {
   function __construct() {
       print "BaseClass 类中构造方法" . PHP_EOL;
   }
}
class SubClass extends BaseClass {
   function __construct() {
       parent::__construct();  // 子类构造方法不能自动调用父类的构造方法
       print "SubClass 类中构造方法" . PHP_EOL;
   }
}
?>
```

# 高级

## 多维数组

```php
<?php 
$sites = array 
( 
    "runoob"=>array 
    ( 
        "菜鸟教程", 
        "http://www.runoob.com" 
    ), 
    "google"=>array 
    ( 
        "Google 搜索", 
        "http://www.google.com" 
    ), 
    "taobao"=>array 
    ( 
        "淘宝", 
        "http://www.taobao.com" 
    ) 
); 
print("<pre>"); // 格式化输出数组 
print_r($sites); 
print("</pre>"); 
?>
```

## 日期

date函数

```php
string date ( string $format [, int $timestamp ] );

```

| 参数      | 描述                                       |
| :-------- | :----------------------------------------- |
| format    | 必需。规定时间戳的格式。                   |
| timestamp | 可选。规定时间戳。默认是当前的日期和时间。 |

```php
<?php
echo date("Y/m/d") . "<br>";
echo date("Y.m.d") . "<br>";
echo date("Y-m-d");
?>
//输出：
2016/10/21
2016.10.21
2016-10-21
```

format完整参数：自己查吧。

## 包含

include  require 导入一个php文件并自动运行

**区别：**

- require 一般放在 PHP 文件的最前面，程序在执行前就会先导入要引用的文件；
- include 一般放在程序的流程控制中，当程序执行时碰到才会引用，简化程序的执行流程。

- require 引入的文件有错误时，执行会中断，并返回一个致命错误；
- include 引入的文件有错误时，会继续执行，并返回一个警告。

## 文件

```php
<?php
$file = fopen("test.txt","r");

//执行一些代码

fclose($file);
?> 
```

```php
<?php
$file = fopen("welcome.txt", "r") or exit("无法打开文件!");
// 读取文件每一行，直到文件结尾
while(!feof($file))
{
    echo fgets($file). "<br>";
}
fclose($file);
?> 
    // feof检查是否到文档末尾
    //fgets逐行读取
    
```



| 模式 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| r    | 只读。在文件的开头开始。                                     |
| r+   | 读/写。在文件的开头开始。                                    |
| w    | 只写。打开并清空文件的内容；如果文件不存在，则创建新文件。   |
| w+   | 读/写。打开并清空文件的内容；如果文件不存在，则创建新文件。  |
| a    | 追加。打开并向文件末尾进行写操作，如果文件不存在，则创建新文件。 |
| a+   | 读/追加。通过向文件末尾写内容，来保持文件内容。              |
| x    | 只写。创建新文件。如果文件已存在，则返回 FALSE 和一个错误。  |
| x+   | 读/写。创建新文件。如果文件已存在，则返回 FALSE 和一个错误。 |

## 文件上传

https://www.runoob.com/php/php-file-upload.html

## Cookie

### 设置cookie

```php
<?php
$expire=time()+60*60*24*30;
setcookie("key", "value", $expire);
?>

<html>
.....
```

**注释：**setcookie() 函数必须位于 <html> 标签之前。

**注释：**在发送 cookie 时，cookie 的值会自动进行 URL 编码，在取回时进行自动解码。（为防止 URL 编码，请使用 setrawcookie() 取而代之。）

### 取cookie

```php
<?php
// 输出 cookie 值
echo $_COOKIE["user"];

// 查看所有 cookie
print_r($_COOKIE);
?>
```

isset()函数用于判断是否有cookie

```php
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>

<?php
if (isset($_COOKIE["user"]))
    echo "欢迎 " . $_COOKIE["user"] . "!<br>";
else
    echo "普通访客!<br>";
?>

</body>
</html>
```

### 删除cookie

设过期日期为过去的时间点

```php
<?php
// 设置 cookie 过期时间为过去 1 小时
setcookie("user", "", time()-3600);
?>
```



## session

Session 的工作机制是：为每个访客创建一个唯一的 id (UID)，并基于这个 UID 来存储变量。UID 存储在 cookie 中，或者通过 URL 进行传导。

```php+HTML
<?php
session_start();//必须位于html标签之前
//上面的代码会向服务器注册用户的会话，以便您可以开始保存用户信息，同时会为用户会话分配一个 UID。
 
if(isset($_SESSION['views']))
{
    $_SESSION['views']=$_SESSION['views']+1;
}
else
{
    $_SESSION['views']=1;
}
//isset() 函数检测是否已设置 "views" 变量。如果已设置 "views" 变量，我们累加计数器。如果 "views" 不存在，则创建 "views" 变量，并把它设置为 1
echo "浏览量：". $_SESSION['views'];
?>
<html>
<body>
 
</body>
</html>
```

### 销毁session

```php
if(isset($_SESSION['views']))
{
    unset($_SESSION['views']);//销毁指定变量
}

session_destroy();//彻底销毁
```

## E-mail

https://www.runoob.com/php/php-secure-mail.html

## 异常处理 

### Error

https://www.runoob.com/php/php-error.html

### Exception

```php
<?php
// 创建一个有异常处理的函数
function checkNum($number)
{
    if($number>1)
    {
        throw new Exception("变量值必须小于等于 1");
    }
        return true;
}
    
// 在 try 块 触发异常
try
{
    checkNum(2);
    // 如果抛出异常，以下文本不会输出
    echo '如果输出该内容，说明 $number 变量';
}
// 捕获异常
catch(Exception $e)
{
    echo 'Message: ' .$e->getMessage();
}
?>
    
    //输出：  Message: 变量值必须小于等于 1
```

#### 自定义Exception类

```php
<?php
class customException extends Exception
{
    public function errorMessage()
    {
        // 错误信息
        $errorMsg = '错误行号 '.$this->getLine().' in '.$this->getFile()
        .': <b>'.$this->getMessage().'</b> 不是一个合法的 E-Mail 地址';
        return $errorMsg;
    }
}
 
$email = "someone@example...com";
 
try
{
    // 检测邮箱
    if(filter_var($email, FILTER_VALIDATE_EMAIL) === FALSE)
    {
        // 如果是个不合法的邮箱地址，抛出异常
        throw new customException($email);
    }
}
 
catch (customException $e)
{
//display custom message
echo $e->errorMessage();
}
?>
```

#### 异常 的规则

- 需要进行异常处理的代码应该放入 try 代码块内，以便捕获潜在的异常。
- 每个 try 或 throw 代码块必须至少拥有一个对应的 catch 代码块。
- 使用多个 catch 代码块可以捕获不同种类的异常。
- 可以在 try 代码块内的 catch 代码块中抛出（再次抛出）异常。

#### 顶层异常处理器

```php
<?php
function myException($exception)
{
    echo "<b>Exception:</b> " , $exception->getMessage();
}
 
set_exception_handler('myException');
 
throw new Exception('Uncaught Exception occurred');
?>
    //输出： Exception: Uncaught Exception occurred
```

在上面的代码中，不存在 "catch" 代码块，而是触发顶层的异常处理程序。此函数来捕获所有未被捕获（catch）的异常。

## 过滤器

https://www.runoob.com/php/php-filter.html

验证过滤外部数据。

什么是外部数据？

- 来自表单的输入数据
- Cookies
- Web services data
- 服务器变量
- 数据库查询结果

## JSON

### 编码

json_encode函数，失败则返回false。

```php
<?php
   $arr = array('a' => 1, 'b' => 2, 'c' => 3, 'd' => 4, 'e' => 5);
   echo json_encode($arr);
?>
//输出：{"a":1,"b":2,"c":3,"d":4,"e":5}

    <?php
   class Emp {
       public $name = "";
       public $hobbies  = "";
       public $birthdate = "";
   }
   $e = new Emp();
   $e->name = "sachin";
   $e->hobbies  = "sports";
   $e->birthdate = date('m/d/Y h:i:s a', "8/5/1974 12:20:03 p");
   $e->birthdate = date('m/d/Y h:i:s a', strtotime("8/5/1974 12:20:03"));

   echo json_encode($e);
?>
//输出：{"name":"sachin","hobbies":"sports","birthdate":"08\/05\/1974 12:20:03 pm"}
```

中文编码问题

```php
<?php
   $arr = array('runoob' => '菜鸟教程', 'taobao' => '淘宝网');
   echo json_encode($arr); // 编码中文
   echo PHP_EOL;  // 换行符
   echo json_encode($arr, JSON_UNESCAPED_UNICODE);  // 不编码中文
?>
//输出：
{"runoob":"\u83dc\u9e1f\u6559\u7a0b","taobao":"\u6dd8\u5b9d\u7f51"}
{"runoob":"菜鸟教程","taobao":"淘宝网"}
```

### 解码

```php
<?php
   $json = '{"a":1,"b":2,"c":3,"d":4,"e":5}';

   var_dump(json_decode($json));
   var_dump(json_decode($json, true));
?>
    //输出：
object(stdClass)#1 (5) {
    ["a"] => int(1)
    ["b"] => int(2)
    ["c"] => int(3)
    ["d"] => int(4)
    ["e"] => int(5)
}

array(5) {
    ["a"] => int(1)
    ["b"] => int(2)
    ["c"] => int(3)
    ["d"] => int(4)
    ["e"] => int(5)
}
```

# 操作MySQL

## 连接

PHP 5 及以上版本建议使用以下方式连接 MySQL :



- **MySQLi extension** ("i" 意为 improved)
- **PDO (PHP Data Objects)**

PDO 应用在 12 种不同数据库中， MySQLi 只针对 MySQL 数据库。

所以，如果你的项目需要在多种数据库中切换，建议使用 PDO ，这样你只需要修改连接字符串和部分查询语句即可。 使用 MySQLi, 如果不同数据库，你需要重新编写所有代码，包括查询。

两者都是面向对象, 但 MySQLi 还提供了 API 接口。

两者都支持预处理语句。 预处理语句可以防止 SQL 注入，对于 web 项目的安全性是非常重要的。

### MySQLi 安装

 Linux 和 Windows:  在 php5 mysql 包安装时 MySQLi 扩展多数情况下是自动安装的。

安装详细信息，请查看： [ http://php.net/manual/en/mysqli.installation.php](http://php.net/manual/en/mysqli.installation.php)

### PDO 安装

 安装详细信息，请查看： [ http://php.net/manual/en/pdo.installation.php](http://php.net/manual/en/pdo.installation.php)

### 连接

mysqli 面向对象

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "";
$port = 3306;
 
// 创建连接
$conn = new mysqli($servername, $username, $password.$dbname,$port);
 
// 检测连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
} 
echo "连接成功";

$conn->close();//关闭连接
?>
```

mysqli面向过程

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "";
$port = "3306";
// 创建连接
$conn = mysqli_connect($servername, $username, $password,$dbname,$port);
 
// 检测连接
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
echo "连接成功";

mysqli_close($conn);//关闭
?>

```

PDO

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
 
try {
    $conn = new PDO("mysql:host=$servername;", $username, $password);
    echo "连接成功"; 
}
catch(PDOException $e)
{
    echo $e->getMessage();
}

$conn=null;//关闭
?>
```

## 创建数据库

### mysqli面向对象

```php
// 创建数据库
$sql = "CREATE DATABASE myDB";
if ($conn->query($sql) === TRUE) {
    echo "数据库创建成功";
} else {
    echo "Error creating database: " . $conn->error;
}
```

### mysqli面向过程

```php
// 创建数据库
$sql = "CREATE DATABASE myDB";
if (mysqli_query($conn, $sql)) {
    echo "数据库创建成功";
} else {
    echo "Error creating database: " . mysqli_error($conn);
}
```

### PDO

```php
$servername = "localhost"; 
$username = "username"; 
$password = "password"; 

try { 
    $conn = new PDO("mysql:host=$servername", $username, $password); 

    // 设置 PDO 错误模式为异常 
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION); 
    $sql = "CREATE DATABASE myDBPDO"; 

    // 使用 exec() ，因为没有结果返回 
    $conn->exec($sql); 

    echo "数据库创建成功<br>"; 
} 
catch(PDOException $e) 
{ 
    echo $sql . "<br>" . $e->getMessage(); 
} 

$conn = null; 
```

使用 PDO 的最大好处是在数据库查询过程出现问题时可以使用异常类来 处理问题。如果  try{ } 代码块出现异常，脚本会停止执行并会跳到第一个 catch(){ } 代码块执行代码。 在以上捕获的代码块中我们输出了 SQL 语句并生成错误信息。 

#### 补充：数据库连接编码问题

```php
//php7
mysqli_query($conn, "set character set 'utf8'");//读库 
mysqli_query($conn,"set names 'utf8'");//写库
//php5
mysql_query("set character set 'utf8'");//读库 
mysql_query("set names 'utf8'");//写库
```

## 创建数据表

只需将上述代码中的$sql改为

```php
  // 使用 sql 创建数据表
    $sql = "CREATE TABLE MyGuests (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY, 
    firstname VARCHAR(30) NOT NULL,
    lastname VARCHAR(30) NOT NULL,
    email VARCHAR(50),
    reg_date TIMESTAMP
    )";
```

## 插入数据

### mysqli面向对象

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDB";
 
// 创建连接
$conn = new mysqli($servername, $username, $password, $dbname);
// 检测连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
} 
 
$sql = "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('John', 'Doe', 'john@example.com')";
 
if ($conn->query($sql) === TRUE) {
    echo "新记录插入成功";
} else {
    echo "Error: " . $sql . "<br>" . $conn->error;
}

//插入多条数据
$sql = "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('John', 'Doe', 'john@example.com');";
$sql .= "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('Mary', 'Moe', 'mary@example.com');";
$sql .= "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('Julie', 'Dooley', 'julie@example.com')";
 
if ($conn->multi_query($sql) === TRUE) {
    echo "新记录插入成功";
} else {
    echo "Error: " . $sql . "<br>" . $conn->error;
}

 
$conn->close();
?>
```

### mysqli面向过程

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDB";
 
// 创建连接
$conn = mysqli_connect($servername, $username, $password, $dbname);
// 检测连接
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
 
$sql = "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('John', 'Doe', 'john@example.com')";
 
if (mysqli_query($conn, $sql)) {
    echo "新记录插入成功";
} else {
    echo "Error: " . $sql . "<br>" . mysqli_error($conn);
}

//插入多条数据
$sql = "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('John', 'Doe', 'john@example.com');";
$sql .= "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('Mary', 'Moe', 'mary@example.com');";
$sql .= "INSERT INTO MyGuests (firstname, lastname, email)
VALUES ('Julie', 'Dooley', 'julie@example.com')";
 
if (mysqli_multi_query($conn, $sql)) {
    echo "新记录插入成功";
} else {
    echo "Error: " . $sql . "<br>" . mysqli_error($conn);
}

 
mysqli_close($conn);
?>
```

### PDO面向对象

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDBPDO";
 
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    // 设置 PDO 错误模式，用于抛出异常
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    //写sql语句
    $sql = "INSERT INTO MyGuests (firstname, lastname, email)
    VALUES ('John', 'Doe', 'john@example.com')";
    // 使用 exec() ，没有结果返回 
    $conn->exec($sql);
    echo "新记录插入成功";
}
catch(PDOException $e)
{
    echo $sql . "<br>" . $e->getMessage();
}

//插入多条
// 开始事务
    $conn->beginTransaction();
    // SQL 语句
    $conn->exec("INSERT INTO MyGuests (firstname, lastname, email) 
    VALUES ('John', 'Doe', 'john@example.com')");
    $conn->exec("INSERT INTO MyGuests (firstname, lastname, email) 
    VALUES ('Mary', 'Moe', 'mary@example.com')");
    $conn->exec("INSERT INTO MyGuests (firstname, lastname, email) 
    VALUES ('Julie', 'Dooley', 'julie@example.com')");
 
    // 提交事务
    $conn->commit();
    echo "新记录插入成功";
}
catch(PDOException $e)
{
    // 如果执行失败回滚
    $conn->rollback();
    echo $sql . "<br>" . $e->getMessage();
}

 
$conn = null;
?>
```

## 预处理语句

本质上就是将一个sql语句中的参数用占位符代替，执行时需要即时传入参数，可以避免重复写sql语句。

相比于直接执行SQL语句，预处理语句有两个主要优点： 

- 预处理语句大大减少了分析时间，只做了一次查询（虽然语句多次执行）。
- 绑定参数减少了服务器带宽，你只需要发送查询的参数，而不是整个语句。
- 预处理语句针对SQL注入是非常有用的，因为参数值发送后使用不同的协议，保证了数据的合法性。

### mysqli预处理

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDB";
 
// 创建连接
$conn = new mysqli($servername, $username, $password, $dbname);
 
// 检测连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
}
 
// 预处理及绑定
$stmt = $conn->prepare("INSERT INTO MyGuests (firstname, lastname, email) VALUES (?, ?, ?)");
$stmt->bind_param("sss", $firstname, $lastname, $email);
 
// 设置参数并执行
$firstname = "John";
$lastname = "Doe";
$email = "john@example.com";
$stmt->execute();
 
$firstname = "Mary";
$lastname = "Moe";
$email = "mary@example.com";
$stmt->execute();
 
$firstname = "Julie";
$lastname = "Dooley";
$email = "julie@example.com";
$stmt->execute();
 
echo "新记录插入成功";
 
$stmt->close();
$conn->close();
?>
```

注意：bind_param函数

该函数绑定了 SQL 的参数，且告诉数据库参数的值。 "sss" 参数列处理其余参数的数据类型。s 字符告诉数据库该参数为字符串。

参数有以下四种类型:

- i - integer（整型）
- d - double（双精度浮点型）
- s -  string（字符串）
- b - BLOB（binary large object:二进制大对象）

### PDO预处理

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDBPDO";
 
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    // 设置 PDO 错误模式为异常
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
 
    // 预处理 SQL 并绑定参数
    $stmt = $conn->prepare("INSERT INTO MyGuests (firstname, lastname, email) 
    VALUES (:firstname, :lastname, :email)");
    $stmt->bindParam(':firstname', $firstname);
    $stmt->bindParam(':lastname', $lastname);
    $stmt->bindParam(':email', $email);
 
    // 插入行
    $firstname = "John";
    $lastname = "Doe";
    $email = "john@example.com";
    $stmt->execute();
 
    // 插入其他行
    $firstname = "Mary";
    $lastname = "Moe";
    $email = "mary@example.com";
    $stmt->execute();
 
    // 插入其他行
    $firstname = "Julie";
    $lastname = "Dooley";
    $email = "julie@example.com";
    $stmt->execute();
 
    echo "新记录插入成功";
}
catch(PDOException $e)
{
    echo "Error: " . $e->getMessage();
}
$conn = null;
?>
```



## 读取数据

### mysqli

```php

<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDB";
 
// 创建连接
$conn = new mysqli($servername, $username, $password, $dbname);
// Check connection
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
} 
 
$sql = "SELECT id, firstname, lastname FROM MyGuests";
$result = $conn->query($sql);
 
if ($result->num_rows > 0) {
    // 输出数据
    while($row = $result->fetch_assoc()) {
        echo "id: " . $row["id"]. " - Name: " . $row["firstname"]. " " . $row["lastname"]. "<br>";
    }
} else {
    echo "0 结果";
}
$conn->close();
?>
```

### mysqli面向过程

```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDB";
$port = 3306;//这个不要忘了，否则连接失败

// 创建连接
$conn = mysqli_connect($servername, $username, $password, $dbname,$port);
// Check connection
if (!$conn) {
    die("连接失败: " . mysqli_connect_error());
}
 
$sql = "SELECT id, firstname, lastname FROM MyGuests";
$result = mysqli_query($conn, $sql);
 
if (mysqli_num_rows($result) > 0) {
    // 输出数据
    while($row = mysqli_fetch_assoc($result)) {
        echo "id: " . $row["id"]. " - Name: " . $row["firstname"]. " " . $row["lastname"]. "<br>";
    }
} else {
    echo "0 结果";
}
 
mysqli_close($conn);
?>
```

### PDO+预处理

```php
<?php
echo "<table style='border: solid 1px black;'>";
echo "<tr><th>Id</th><th>Firstname</th><th>Lastname</th></tr>";
 
class TableRows extends RecursiveIteratorIterator {
    function __construct($it) { 
        parent::__construct($it, self::LEAVES_ONLY); 
    }
 
    function current() {
        return "<td style='width:150px;border:1px solid black;'>" . parent::current(). "</td>";
    }
 
    function beginChildren() { 
        echo "<tr>"; 
    } 
 
    function endChildren() { 
        echo "</tr>" . "\n";
    } 
} 
 
$servername = "localhost";
$username = "username";
$password = "password";
$dbname = "myDBPDO";
 
try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SELECT id, firstname, lastname FROM MyGuests"); 
    $stmt->execute();
 
    // 设置结果集为关联数组
    $result = $stmt->setFetchMode(PDO::FETCH_ASSOC); 
    foreach(new TableRows(new RecursiveArrayIterator($stmt->fetchAll())) as $k=>$v) { 
        echo $v;
    }
}
catch(PDOException $e) {
    echo "Error: " . $e->getMessage();
}
$conn = null;
echo "</table>";
?>
```

## where

```php
<?php
$con=mysqli_connect("localhost","username","password","database");
// 检测连接
if (mysqli_connect_errno())
{
    echo "连接失败: " . mysqli_connect_error();
}

$result = mysqli_query($con,"SELECT * FROM Persons
WHERE FirstName='Peter'");

while($row = mysqli_fetch_array($result))
{
    echo $row['FirstName'] . " " . $row['LastName'];
    echo "<br>";
}
?>
```

## order by

```php
<?php
$con=mysqli_connect("localhost","username","password","database");
// 检测连接
if (mysqli_connect_errno())
{
    echo "连接失败: " . mysqli_connect_error();
}

$result = mysqli_query($con,"SELECT * FROM Persons ORDER BY age");

while($row = mysqli_fetch_array($result))
{
    echo $row['FirstName'];
    echo " " . $row['LastName'];
    echo " " . $row['Age'];
    echo "<br>";
}

mysqli_close($con);
?>
```

## update & delete

```php
$con=mysqli_connect("localhost","username","password","database");
// 检测连接
if (mysqli_connect_errno())
{
    echo "连接失败: " . mysqli_connect_error();
}

mysqli_query($con,"UPDATE Persons SET Age=36
WHERE FirstName='Peter' AND LastName='Griffin'");

mysqli_close($con);
```

```php
<?php
$con=mysqli_connect("localhost","username","password","database");
// 检测连接
if (mysqli_connect_errno())
{
    echo "连接失败: " . mysqli_connect_error();
}

mysqli_query($con,"DELETE FROM Persons WHERE LastName='Griffin'");

mysqli_close($con);
?>
```

## ODBC

```php
<?php
# 连接到数据源，四个参数：数据源名、用户名、密码、可选的指针类型
$conn=odbc_connect('northwind','','');
if (!$conn)
{
    exit("连接失败: " . $conn);
}

$sql="SELECT * FROM customers";
$rs=odbc_exec($conn,$sql); # 执行sql语句

if (!$rs)
{
    exit("SQL 语句错误");
}
echo "<table><tr>";
echo "<th>Companyname</th>";
echo "<th>Contactname</th></tr>";

while (odbc_fetch_row($rs)) //odbc_fetch_row函数用于从结果集中返回记录，如果能够返回则为true，否则为false
{
	# 查找字段为“CompanyName”的值
    $compname=odbc_result($rs,"CompanyName");
    $conname=odbc_result($rs,"ContactName");
    echo "<tr><td>$compname</td>";
    echo "<td>$conname</td></tr>";
}
odbc_close($conn); # 关闭ODBC连接
echo "</table>";
?>
```


