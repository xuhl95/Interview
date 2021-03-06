## PHP 篇

### echo、print、print_r、var_dump 区别

> `echo`和`print`是语言结构、`print_r`和`var_dump`是普通函数

- echo：输出一个或多个字符串

- print：输出字符串

- print_r：打印关于变量的易于理解的信息

- var_dump：打印关于变量的易于理解的信息(带类型)

拓展阅读 [《echo、print、print_r、var_dump区别》](./03.echo、print、print_r、var_dump区别.md)

### 单引号和双引号的区别

双引号可以被分析器解析，单引号则不行

### isset 和 empty 的区别

isset：检测变量是否已设置并且非 NULL

empty：判断变量是否为空，变量为 0/false 也会被认为是空；变量不存在，不会产生警告

### array_merge和+号区别

结论:用加号合并数组既考虑数字索引的键值对,也考虑字符串索引的键值对,用前边数组的值覆盖后边的键名相同的值;

用array_merge()合并数组:只考虑字符串索引的键值对,用后边数组的值覆盖掉前面数组中键名相同的值,数字索引的值则不覆盖,同时保留

另外:array_merge()会重排两个数组的数字索引,"+"则不会

```
<?php
$a = array('r'=>1,2,3,4);
var_dump('a',$a);
$b = array('r'=>5,6,7,8);
var_dump('b',$b);
$c = array('r'=>5,6,7,8,9);
var_dump('c',$c);

var_dump('a+b',$a+$b);
var_dump('a+c', $a+$c);

var_dump('amb', array_merge($a, $b));
var_dump('amc', array_merge($a, $c));

string 'a' (length=1)
array
  'r' => int 1
=> int 2
=> int 3
=> int 4
string 'b' (length=1)
array
  'r' => int 5
=> int 6
=> int 7
=> int 8
string 'c' (length=1)
array
  'r' => int 5
=> int 6
=> int 7
=> int 8
=> int 9

string 'a+b' (length=3)
array
  'r' => int 1
=> int 2
=> int 3
=> int 4

string 'a+c' (length=3)
array
  'r' => int 1
=> int 2
=> int 3
=> int 4
=> int 9

string 'amb' (length=3)
array
  'r' => int 5
=> int 2
=> int 3
=> int 4
=> int 6
=> int 7
=> int 8

string 'amc' (length=3)
array
  'r' => int 5
=> int 2
=> int 3
=> int 4
=> int 6
=> int 7
=> int 8
=> int 9
```

### static、self、$this 的区别

static：static 可以用于静态或非静态方法中，也可以访问类的静态属性、静态方法、常量和非静态方法，但不能访问非静态属性

self：可以用于访问类的静态属性、静态方法和常量，但 self 指向的是当前定义所在的类，这是 self 的限制

$this：指向的是实际调用时的对象，也就是说，实际运行过程中，谁调用了类的属性或方法，$this 指向的就是哪个对象。但 $this 不能访问类的静态属性和常量，且 $this 不能存在于静态方法中

### static:: 与self::区别

使用self::或者__CLASS__对当前类的静态引用，取决于定义当前方法所在的类：

使用 static::不再被解析为定义当前方法所在的类，而是在实际运行时计算的。也可以称之为“静态绑定”，因为它可以用于（但不限于）静态方法的调用。

静态绑定是PHP 5.3.0，增加的一个功能 用于在继承范围内引用静态调用的类

简单通俗的来说, 

self就是写在哪个类里面, 实际调用的就是这个类.

static代表使用的这个类, 就是你在父类里写的static,然后被子类覆盖，使用的就是子类的方法或属性

php手册解释 [《Static（静态）关键字》](https://www.php.net/manual/zh/language.oop5.static.php)

### include、require、include_once、require_once 的区别

require 和 include 几乎完全一样，除了处理失败的方式不同之外。require 在出错时产生 E_COMPILE_ERROR 级别的错误。换句话说将导致脚本中止而 include 只产生警告（E_WARNING），脚本会继续运行

include_once 语句在脚本执行期间包含并运行指定文件。此行为和 include 语句类似，唯一区别是如果该文件中已经被包含过，则不会再次包含。如同此语句名字暗示的那样，只会包含一次

### 数组处理函数

更多数组函数 [《更多数组函数》](https://www.php.net/manual/zh/book.array.php)

array_count_values — 统计数组中所有的值

array_flip — 交换数组中的键和值

array_merge — 合并一个或多个数组

array_multisort — 对多个数组或多维数组进行排序

array_pad — 以指定长度将一个值填充进数组

array_pop — 弹出数组最后一个单元(出栈)

array_push — 将一个或多个单元压入数组的末尾(入栈)

array_rand — 从数组中随机(伪随机)取出一个或多个单元

array_keys — 返回数组中部分的或所有的键名

array_values — 返回数组中所有的值

count — 计算数组中的单元数目，或对象中的属性个数

sort — 对数组排序


### 字符串处理函数

更多字符串函数 [《字符串函数》](https://www.php.net/manual/zh/ref.strings.php)

chunk_split — 将字符串分割成小块

### Cookie 和 Session

Cookie：PHP 透明的支持 HTTP cookie 。cookie 是一种远程浏览器端存储数据并以此来跟踪和识别用户的机制

Session：会话机制(Session)在 PHP 中用于保持用户连续访问Web应用时的相关数据

原文地址[《COOKIE和SESSION的使用以及区别》](https://www.imooc.com/article/14919)

### cookie和session区别

（1）存储位置：Cookie存储在客户端浏览器中，相对不安全；Session内容所在文件存储在服务器中，一般在根目录下的tmp文件夹中，相对更安全。

（2）数量和大小限制：Cookie存储的数据在不同的浏览器会有不同的限制，一般在同一个域名下，Cookie变量数量控制在20个以内，每个cookie值的大小控制在4kb以内。session值没有大小和数量限制，但如果数量过多，会增大服务器的压力。

（3）内容区别：cookie保存的内容是字符串，而服务器中的session保存的数据是对象。

（4）路径区别：session不能区分路径，同一个用户在访问一个网站期间，所有的session在任何一个地方都可以访问到；而cookie中如果设置了路径参数，那么同一个网站中不同路径下的cookie互相是访问不到的。

#### 抽象类

抽象类应用的定义如下：

```
abstract class ClassName{
}
```

抽象类具有以下特点：
1）定义一些方法，子类必须实现父类所有的抽象方法，只有这样，子类才能被实例化，
否则子类还是一个抽象类。

2）抽象类不能被实例化，它的意义在于被扩展。

3）抽象方法不必实现具体的功能，由子类来完成。

4）当子类实现抽象类的方法时，这些方法的访问控制可以和父类中的一样，也可以有更
高的可见性，但是不能有更低的可见性。例如，某个抽象方法被声明为 protected 的，那么子
类中实现的方法就应该声明为 protected 或者 public 的，而不能声明为 private。

5）如果抽象方法有参数，那么子类的实现也必须有相同的参数个数，必须匹配。但有一
个例外：子类可以定义一个可选参数（这个可选参数必须要有默认值），即使父类抽象方法
的声明里没有这个参数，两者的声明也无冲突。下面通过一个例子来加深理解：

```
<?php
<?php
abstract classA{
abstract protected function greet($name);
}
class B extendsA{
public function greet($name, $how="Hello ") {
echo $how.$name."\n";
}

}
$b = new B;
$b->greet("James");
$b->greet("James","Good morning ");
?>
程序的运行结果为
Hello James
Good morning James
```

定义抽象类时，通常需要遵循以下规则：

1）一个类只要含有至少一个抽象方法，就必须声明为抽象类。

2）抽象方法不能够含有方法体。

#### 接口

接口可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。在 PHP 中，
接口是通过 interface 关键字来实现的，与定义一个类类似，唯一不同的是接口中定义的方法
都是公有的而且方法都没有方法体。接口中所有的方法都是公有的，此外接口中还可以定义
常量。接口常量和类常量的使用完全相同，但是不能被子类或子接口所覆盖。要实现一个接
口，可以通过关键字 implements 来完成。实现接口的类中必须实现接口中定义的所有方法。
虽然 PHP 不支持多重继承，但是一个类可以实现多个接口，用逗号来分隔多个接口的名称。

#### 接口和抽象类主要有以下区别：

[抽象类和接口的区别，使用场景](https://blog.csdn.net/sinat_36187124/article/details/78684406)

抽象类：PHP5 支持抽象类和抽象方法。被定义为抽象的类不能被实例化。任何一个类，
如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。被定义
为抽象的方法只是声明了其调用方法和参数，不能定义其具体的功能实现。抽象类通过关键
字 abstract 来声明。

接口：可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。在这种
情况下，可以通过 interface 关键字来定义一个接口，在接口中声明的方法都不能有方法体。

二者虽然都是定义了抽象的方法，但是事实上两者区别还是很大的，主要区别如下：

1）对接口的实现是通过关键字 implements 来实现的，而抽象类继承则是使用类继承的关
键字 extends 实现的。

2）接口没有数据成员（可以有常量），但是抽象类有数据成员（各种类型的成员变量），
抽象类可以实现数据的封装。

3）接口没有构造函数，抽象类可以有构造函数。

4）接口中的方法都是 public 类型，而抽象类中的方法可以使用 private、protected 或public 来修饰。

5）一个类可以同时实现多个接口，但是只能实现一个抽象类。

### final有什么作用

[官方解释](https://www.php.net/manual/zh/language.oop5.final.php)

final 可以用来修饰类和方法，当修饰类的时候，该类不能被继承，当修饰方法的时候，该方法不能被覆盖。

具体而言，final 具有以下特性：

final用于声明方法和类，分别表示方法不可被覆盖、类不可被继承（不能再派生出新的子类）。

final方法：当一个方法被声明为final 时，不允许任何子类重写这个方法，但子类仍然可
以使用这个方法。需要注意的是，final不能修饰类的成员变量。

final类：当一个类被声明为final时，此类不能被继承，所有方法都不能被重写。值得注
意的是，一个类不能既被声明为 abstract，又被声明为final。

### 预定义变量

对于全部脚本而言，PHP 提供了大量的预定义变量

超全局变量 — 超全局变量是在全部作用域中始终可用的内置变量

```text
$GLOBALS — 引用全局作用域中可用的全部变量
$_SERVER — 服务器和执行环境信息
$_GET — HTTP GET 变量
$_POST — HTTP POST 变量
$_FILES — HTTP 文件上传变量
$_REQUEST — HTTP Request 变量
$_SESSION — Session 变量
$_ENV — 环境变量
$_COOKIE — HTTP Cookies
$php_errormsg — 前一个错误信息
$HTTP_RAW_POST_DATA — 原生POST数据
$http_response_header — HTTP 响应头
$argc — 传递给脚本的参数数目
$argv — 传递给脚本的参数数组
```

- 超全局变量

PHP 中的许多预定义变量都是“超全局的”，这意味着它们在一个脚本的全部作用域中都可用。在函数或方法中无需执行 global $variable; 就可以访问它们

超全局变量：$GLOBALS、$\_SERVER、$\_GET、$\_POST、$\_FILES、$\_COOKIE、$\_SESSION、$\_REQUEST、$\_ENV

### 传值和传引用的区别

传值导致对象生成了一个拷贝，传引用则可以用两个变量指向同一个内容

### 构造函数和析构函数

[官方解释](https://www.php.net/manual/zh/language.oop5.decon.php)

构造函数：PHP 5 允行开发者在一个类中定义一个方法作为构造函数。具有构造函数的类会在每次创建新对象时先调用此方法，所以非常适合在使用对象之前做一些初始化工作

析构函数：PHP 5 引入了析构函数的概念，这类似于其它面向对象的语言，如 C++。析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行

### 魔术方法

[魔术方法](https://www.php.net/manual/zh/language.oop5.magic.php)

\_\_construct()， \_\_destruct()， \_\_call()， \_\_callStatic()， \_\_get()， \_\_set()， \_\_isset()， \_\_unset()， \_\_sleep()， \_\_wakeup()， \_\_toString()， \_\_invoke() 等方法在 PHP 中被称为"魔术方法"（Magic methods）

### public、protected、private、final区别

对属性或方法的访问控制，是通过在前面添加关键字 public（公有），protected（受保护）或 private（私有）来实现的。被定义为公有的类成员可以在任何地方被访问

PHP 5 新增了一个 final 关键字。如果父类中的方法被声明为 final，则子类无法覆盖该方法。如果一个类被声明为 final，则不能被继承

### 客户端/服务端 IP 获取，了解代理透传 实际IP 的概念

客户端IP: $\_SERVER['REMOTE_ADDR']

服务端IP: $\_SERVER['SERVER_ADDR']

客户端IP(代理透传): $\_SERVER['HTTP_X_FORWARDED_FOR']

### PHP_$_SERVER_说明详解

[$_SERVER详解](https://www.php.net/manual/zh/reserved.variables.server.php)

$_SERVER['PHP_SELF'] #当前正在执行 脚本的文件名，与 document root相关。

$_SERVER['argv'] #传递给该 脚本的参数。

$_SERVER['argc'] #包含传递给程序的 命令行参数的个数（如果运行在命令行模式）。

$_SERVER['GATEWAY_INTERFACE'] #服务器使用的 CGI 规范的版本。例如，“CGI/1.1”。

$_SERVER['SERVER_NAME'] #当前 运行脚本所在服务器 主机的名称。

$_SERVER['SERVER_SOFTWARE'] #服务器标识的字串，在响应请求时的头部中给出。

$_SERVER['SERVER_PROTOCOL'] #请求页面时通信协议的名称和版本。例如，“HTTP/1.0”。

$_SERVER['REQUEST_METHOD'] #访问页面时的请求方法。例如：“GET”、“HEAD”，“POST”，“PUT”。

$_SERVER['QUERY_STRING'] #查询(query)的字符串。

$_SERVER['DOCUMENT_ROOT'] #当前 运行脚本所在的文档根目录。在服务器配置文件中定义。

$_SERVER['HTTP_ACCEPT'] #当前请求的 Accept: 头部的内容。

$_SERVER['HTTP_ACCEPT_CHARSET'] #当前请求的 Accept-Charset: 头部的内容。例如：“iso-8859-1,*,utf-8”。

$_SERVER['HTTP_ACCEPT_ENCODING'] #当前请求的 Accept-Encoding: 头部的内容。例如：“gzip”。

$_SERVER['HTTP_ACCEPT_LANGUAGE']#当前请求的 Accept-Language: 头部的内容。例如：“en”。

$_SERVER['HTTP_CONNECTION'] #当前请求的 Connection: 头部的内容。例如：“Keep-Alive”。

$_SERVER['HTTP_HOST'] #当前请求的 Host: 头部的内容。

$_SERVER[' HTTP_REFERER'] #链接到当前页面的前一页面的 URL 地址。

$_SERVER[' HTTP_USER_AGENT'] #当前请求的 User-Agent: 头部的内容。

$_SERVER['HTTPS'] — 如果通过https访问,则被设为一个非空的值(on)，否则返回off

$_SERVER['REMOTE_ADDR'] #正在浏览当前页面用户的 IP 地址。

$_SERVER['REMOTE_HOST'] #正在浏览当前页面用户的 主机名。

$_SERVER['REMOTE_PORT'] #用户连接到服务器时所使用的端口。

$_SERVER['SCRIPT_FILENAME'] #当前执行 脚本的 绝对路径名。

$_SERVER['SERVER_ADMIN'] # 管理员信息

$_SERVER['SERVER_PORT'] #服务器所使用的端口

$_SERVER['SERVER_SIGNATURE'] #包含服务器版本和 虚拟主机名的字符串。

$_SERVER['PATH_TRANSLATED'] #当前 脚本所在文件系统（不是文档根目录）的基本路径。

$_SERVER['SCRIPT_NAME'] #包含当前 脚本的路径。这在页面需要指向自己时非常有用。

$_SERVER['REQUEST_URI'] #访问此页面所需的 URI。例如，“/index.html”

### 类的静态调用和实例化调用

- 占用内存

静态方法在内存中只有一份，无论调用多少次，都是共用的

实例化不一样，每一个实例化是一个对象，在内存中是多个的

- 不同点

静态调用不需要实例化即可调用

静态方法不能调用非静态属性，因为非静态属性需要实例化后，存放在对象里

静态方法可以调用非静态方法，使用 self 关键字。php 里，一个方法被 `self::` 后，自动转变为静态方法

调用类的静态函数时不会自动调用类的构造函数

### PHP 不实例化调用方法

静态调用、使用 PHP 反射方式

### php.ini 配置选项

- 配置选项

|名字|默认|备注|
|---|---|---|
|short_open_tag|"1"|是否开启缩写形式(`<? ?>`)|
|precision|"14"|浮点数中显示有效数字的位数|
|disable_functions|""|禁止某些函数|
|disable_classes|""|禁用某些类|
|expose_php|""|是否暴露 PHP 被安装在服务器上|
|max_execution_time|30|最大执行时间|
|memory_limit|128M|每个脚本执行的内存限制|
|error_reporting|NULL|设置错误报告的级别 `E_ALL` & ~`E_NOTICE` & ~`E_STRICT` & ~`E_DEPRECATED`|
|display_errors|"1"|显示错误|
|log_errors|"0"|设置是否将错误日志记录到 error_log 中|
|error_log|NULL|设置脚本错误将被记录到的文件|
|upload_max_filesize|"2M"|最大上传文件大小|
|post_max_size|"8M"|设置POST最大数据限制|

```shell
php -ini | grep short_open_tag //查看 php.ini 配置
```

- 动态设置

```php
ini_set(string $varname , string $newvalue);

ini_set('date.timezone', 'Asia/Shanghai'); //设置时区
ini_set('display_errors', '1'); //设置显示错误
ini_set('memory_limit', '256M'); //设置最大内存限制
```
### 502、504 错误产生原因及解决方式

#### 502

502 表示网关错误，当 PHP-CGI 得到一个无效响应，网关就会输出这个错误

- `php.ini` 的 memory_limit 过小
- `php-fpm.conf` 中 max_children、max_requests 设置不合理
- `php-fpm.conf` 中 request_terminate_timeout、max_execution_time 设置不合理
- php-fpm 进程处理不过来，进程数不足、脚本存在性能问题

#### 504

504 表示网关超时，PHP-CGI 没有在指定时间响应请求，网关将输出这个错误

- Nginx+PHP 架构，可以调整 FastCGI 超时时间，fastcgi_connect_timeout、fastcgi_send_timeout、fastcgi_read_timeout

#### 500

php 代码问题，文件权限问题，资源问题

#### 503

超载或者停机维护

#### 502 503 504

502：nginx在这里充当的是反向代理服务器的角色，是把http协议请求转成fastcgi协议的请求，通过fastcgi_pass指令传递给php-fpm进程，当php-fpm进程响应的内容是nginx无法理解的响应，就会返回502 bad gateway

503：一个http请求占用一个php-fpm进程，瞬时请求量过大时，没有足够的php-fpm进程去处理请求，就会返回503 service unavailable

504: 单个php-fpm进程阻塞超过nginx的时间阈值返回504 gateway timeout

### 如何返回一个301重定向

```php
header('HTTP/1.1 301 Moved Permanently');
header('Location: https://blog.maplemark.cn');
```

### PHP 与 MySQL 连接方式

#### MySQL

```php
$conn = mysql_connect('127.0.0.1:3306', 'root', '123456');
if (!$conn) {
    die(mysql_error() . "\n");
}
mysql_query("SET NAMES 'utf8'");
$select_db = mysql_select_db('app');
if (!$select_db) {
    die(mysql_error() . "\n");
}
$sql = "SELECT * FROM `user` LIMIT 1";
$res = mysql_query($sql);
if (!$res) {
    die(mysql_error() . "\n");
}
while ($row = mysql_fetch_assoc($res)) {
    var_dump($row);
}
mysql_close($conn);
```

#### MySQLi

```php
$conn = @new mysqli('127.0.0.1:3306', 'root', '123456');
if ($conn->connect_errno) {
    die($conn->connect_error . "\n");
}
$conn->query("set names 'utf8';");
$select_db = $conn->select_db('user');
if (!$select_db) {
    die($conn->error . "\n");
}
$sql = "SELECT * FROM `user` LIMIT 1";
$res = $conn->query($sql);
if (!$res) {
    die($conn->error . "\n");
}
while ($row = $res->fetch_assoc()) {
    var_dump($row);
}
$res->free();
$conn->close();
```

#### PDO

```php
$pdo = new PDO('mysql:host=127.0.0.1:3306;dbname=user', 'root', '123456');
$pdo->exec("set names 'utf8'");
$sql = "SELECT * FROM `user` LIMIT 1";
$stmt = $pdo->prepare($sql);
$stmt->bindValue(1, 1, PDO::PARAM_STR);
$rs = $stmt->execute();
if ($rs) {
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        var_dump($row);
    }
}
$pdo = null;
```

### MySQL、MySQLi、PDO 区别

#### MySQL

- 允许 PHP 应用与 MySQL 数据库交互的早期扩展
- 提供了一个面向过程的接口，不支持后期的一些特性

#### MySQLi

- 面向对象接口
- prepared 语句支持
- 多语句执行支持
- 事务支持
- 增强的调试能力

#### PDO

- PHP 应用中的一个数据库抽象层规范
- PDO 提供一个统一的 API 接口，无须关心数据库类型
- 使用标准的 PDO API，可以快速无缝切换数据库

### 数据库持久连接

把 PHP 用作多进程 web 服务器的一个模块，这种方法目前只适用于 Apache。

对于一个多进程的服务器，其典型特征是有一个父进程和一组子进程协调运行，其中实际生成 web 页面的是子进程。每当客户端向父进程提出请求时，该请求会被传递给还没有被其它的客户端请求占用的子进程。这也就是说当相同的客户端第二次向服务端提出请求时，它将有可能被一个不同的子进程来处理。在开启了一个持久连接后，所有请求 SQL 服务的后继页面都能够重用这个已经建立的 SQL Server 连接。

### base64 编码原理

![base64](./assets/php-base64.png)

### ip2long 实现

![ip2long](./assets/php-ip2long.png)

```
124.205.30.150=2093817494

list($p1,$p2,$p3,$p4) = explode(',','124.205.30.150');

$realNum = $p1<<24+$p2<<16+$p3<<8+$p4;
```

### MVC的理解

MVC 包括三类对象。模型 Model 是应用对象，视图 View 是它在屏幕上的表示，控制器 Controller 定义用户界面对用户输入的响应方式。不使用 MVC，用户界面设计往往将这些对象混在一起，而 MVC 则将它们分离以提高灵活性和复用性

### 主流PHP框架特点

#### Laravel

易于访问，功能强大，并提供大型，强大的应用程序所需的工具

- 简单快速的路由引擎
- 强大的依赖注入容器
- 富有表现力，直观的数据库 ORM
- 提供数据库迁移功能
- 灵活的任务调度器
- 实时事件广播

#### Symfony

- Database engine-independent
- Simple to use, in most cases, but still flexible enough to adapt to complex cases
- Based on the premise of convention over configuration--the developer needs to configure only the unconventional
- Compliant with most web best practices and design patterns
- Enterprise-ready--adaptable to existing information technology (IT) policies and architectures, and stable enough for long-term projects
- Very readable code, with phpDocumentor comments, for easy maintenance
- Easy to extend, allowing for integration with other vendor libraries

#### CodeIgniter

- 基于模型-视图-控制器的系统
- 框架比较轻量
- 全功能数据库类，支持多个平台
- Query Builder 数据库支持
- 表单和数据验证
- 安全性和 XSS 过滤
- 全页面缓存

#### ThinkPHP

- 采用容器统一管理对象
- 支持 Facade
- 更易用的路由
- 注解路由支持
- 路由跨域请求支持
- 验证类增强
- 配置和路由目录独立
- 取消系统常量
- 类库别名机制
- 模型和数据库增强
- 依赖注入完善
- 支持 PSR-3 日志规范
- 中间件支持
- 支持 Swoole/Workerman 运行

### 对象关系映射/ORM

#### 优点

- 缩短编码时间、减少甚至免除对 model 的编码，降低数据库学习成本
- 动态的数据表映射，在表结构发生改变时，减少代码修改
- 可以很方便的引入附加功能(cache 层)

#### 缺点

- 映射消耗性能、ORM 对象消耗内存
- SQL 语句较为复杂时，ORM 语法可读性不高(使用原生 SQL)

### 链式调用实现

类定义一个内置变量，让类中其他定义方法可访问到

### 异常处理

set_exception_handler — 设置用户自定义的异常处理函数

使用 try / catch 捕获

### 如何实现异步调用

```php
$fp = fsockopen("blog.maplemark.cn", 80, $errno, $errstr, 30);
if (!$fp) {
    echo "$errstr ($errno)<br />\n";
} else {
    $out = "GET /backend.php  / HTTP/1.1\r\n";
    $out .= "Host: blog.maplemark.cn\r\n";
    $out .= "Connection: Close\r\n\r\n";
    fwrite($fp, $out);
    /*忽略执行结果
    while (!feof($fp)) {
        echo fgets($fp, 128);
    }*/
    fclose($fp);
}
```

### PHP 支持回调的函数，实现一个

array_map、array_filter、array_walk、usort

is_callable + callbacks + 匿名函数实现

### 发起 HTTP 请求有哪几种方式，它们有何区别

cURL、file_get_contents、fopen、fsockopen

### php for while foreach 迭代数组时候，哪个效率最高

### 弱类型变量如何实现

PHP 中声明的变量，在 zend 引擎中都是用结构体 zval 来保存，通过共同体实现弱类型变量声明

### PHP 拓展初始化

- 初始化拓展

```shell
$ php /php-src/ext/ext_skel.php --ext
```

- 定义拓展函数

zend_module_entry 定义 Extension name 编写 PHP_FUNCTION 函数

- 编译安装

```shell
$ phpize $ ./configure $ make && make install
```

### 如何获取扩展安装路径

1、phpinfo() => extensions

2、php -i|grep extensions

### 垃圾回收机制

引用计数器

### yield是什么，说个使用场景 yield、yield 核心原理是什么

一个生成器函数看起来像一个普通的函数，不同的是普通函数返回一个值，而一个生成器可以yield生成许多它所需要的值

拓展阅读 [《yield 核心原理》](./04.yield.md)

### traits 与 interfaces 区别 及 traits 解决了什么痛点
拓展阅读 [《traits 与 interfaces》](06.traits与interfaces的区别及traits解决了什么痛点.md)

### 如何 foreach 迭代对象、如何数组化操作对象 $obj[key]、如何函数化对象 $obj(123);

### Swoole 适用场景，协程实现方式

那你知道swoole的进程模型

### PHP 数组底层实现 （HashTable + Linked list）

### Copy on write 原理，何时 GC


### 字符串、数字比较大小的原理，注意 0 开头的8进制、0x 开头16进制

### BOM头是什么，怎么除去

#### bom头

BOM头是放在UTF-8编码的文件的头部的，占用三个字节，用来标识该文件属于UTF-8编码

#### 去除bom头

```
$result = trim($result, "\xEF\xBB\xBF");
print_r(json_decode($result, true));
exit;
```

### 模板引擎是什么，解决什么问题、实现原理（Smarty、Twig、Blade）

#### 模板引擎是什么

模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。