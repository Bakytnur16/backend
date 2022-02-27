php课程：https://edu.aliyun.com/course/509?utm_content=m_1000015030
https://zhuanlan.zhihu.com/p/24035779

server服务器和client客户端

# PHP 7.4
PHP（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。
PHP 脚本在服务器上执行。    
注释： // /* */

> echo - 可以输出一个或多个字符串(echo 输出的速度比 print 快， echo 没有返回值，print有返回值1)  
> ```echo "这是一个", "字符串，", "使用了", "多个", "参数。";```  
> print - 只允许输出一个字符串，返回值总为 1

## 变量
以 $ 符号开始，区分大小写  
变量有：local，global（函数外部的变量），static（静止不被删除），parameter（函数参数作用域）  
$GLOBALS[index] 全局数组  
 ```
 <?php
$x = 5;
$y = 6;
function myTest()
{
	global $x,$y; // 全局变量，如果要在一个函数中访问一个全局变量，需要使用 global 关键字
	$y += $x;
}

myTest();
echo $y;
?>
 
<?php
$x = 5;
$y = 6;
$z = $x + $y;
echo $z;
?>

<?php    
function myTest()
{
	static $x = 0;
	echo $x;
	$x++;
	echo PHP_EOL;
}

myTest();
myTest();
myTest();
// 输出 0 1 2
?>

参数作用域
function myTest($x){
    echo $x;
}
myTest(5);
```
##超级变量：
```

    $GLOBALS
    $_SERVER: $_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。  
    $_REQUEST: PHP $_REQUEST 用于收集HTML表单提交的数据。  
```
<?php 
$name = $_REQUEST['fname']; 
echo $name; 
?>
```
    $_POST: PHP $_POST 被广泛应用于收集表单数据   
    $_GET
    $_FILES
    $_ENV
    $_COOKIE
    $_SESSION


```

### EOF(heredoc)
是一种在命令行shell（如sh、csh、ksh、bash、PowerShell和zsh）和程序语言（像Perl、PHP、Python和Ruby）里定义一个字符串的方法。
1. 必须后接分号
2. 结束标识必须顶格独自占一行(即必须从行首开始，前后不能衔接任何空白和字符)
```
echo <<<EOF
        <h1>我的第一个标题</h1>
        <p>我的第一个段落。</p>
EOF;

$name="runoob";
$a= <<<EOF
        "abc"$name
        "123"
EOF;
// 结束需要独立一行且前后不能空格
echo $a;
多行输入符
```
# 数据类型   
 PHP var_dump() 函数返回变量的数据类型和值  
1. **String（字符串）** 
并置运算符 (.) 用于把两个字符串值连接起来。
```
$txt1="hello world";
$txt2="what a nice day";
echo $txt1 ." ". $txt2;
hello world what a nice day

echo strlen("hello world!");
echo strpos("hello world!","world");
```
**strlen() 字符串长度  
strpos() 函数用于在字符串内查找一个字符或一段指定的文本,返回第一个匹配的字符位置**  
   
2. **Integer（整型）**
整型可以用三种格式来指定：十进制， 十六进制（ 以 0x 为前缀）或八进制（前缀为 0）  

3. **Float（浮点型）** 可以有指数
4. Boolean（布尔型）
5. **Array（数组）**
```$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
$cars[0]="Volvo";  
echo count($cars);

$z = array("voloe","boolm","toyota");
echo $z[0];
?>

$cars=array("Volvo","BMW","Toyota");


for($x=0;$x<count($cars);$x++){
    echo $cars[$x];
    echo "<br>";
}
```
count() 函数用于返回数组的长度
```
$cars=array("Volvo","BMW","Toyota");
echo count($cars);
// 长度是3
$cars[0]="Volvo";
$cars[1]="BMW";
$cars[2]="Toyota"; 
```
### 数组排列：
```
    sort() - 对数组进行升序排列
    rsort() - 对数组进行降序排列
    asort() - 根据关联数组的值，对数组进行升序排列
    ksort() - 根据关联数组的键，对数组进行升序排列
    arsort() - 根据关联数组的值，对数组进行降序排列
    krsort() - 根据关联数组的键，对数组进行降序排列
```

### 关联数组（像字典）  
```$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43"; ```

 ```
 $age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
 $age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
foreach($age as $x => $y){
    echo "key=" .$x.", value=" . $y;
}
 
 ```
 

6. Object（对象）
7. NULL（空值）

**松散比较：使用两个等号 == 比较，只比较值，不比较类型。x <> y 不等**  
**严格比较：用三个等号 === 比较，除了比较值，也比较类型。x !== y 绝对不等**  

```
<?php
if (42 == '42'){
	echo '1. 值相等';
}
echo PHP_EOL; //换行符

if(42 === '42'){
	echo '2.类型相等';
}else{
	echo '3.类型不相等';
	}
?>

"0" == null: bool(false)
"0" === null: bool(false)

```

### 常量
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
name：必选参数，常量名称，即标志符。  
value：必选参数，常量的值。  
case_insensitive ：可选参数，如果设置为 TRUE，该常量则大小写不敏感。默认是大小写敏感的。  

**常量:** 
```
__LINE__: 文件中的当前行号。  

<?php
echo '这是第 " '  . __LINE__ . ' " 行'; //这是第 “ 2 ” 行
?>

__FILE__: 文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。  


<?php
echo '该文件位于 " '  . __FILE__ . ' " ';
?>

__DIR__:文件所在的目录。


<?php
echo '该文件位于 " '  . __DIR__ . ' " ';
?>

__FUNCTION__:函数名称（PHP 4.3.0 新加）

function test() {
    echo  '函数名为：' . __FUNCTION__ ;
}
test();

__CLASS__:类的名称  
__METHOD__:类的方法名  
__NAMESPACE__:当前命名空间的名称（区分大小写）

```


++ x	预递增	x 加 1，然后返回 x  
x ++	后递增	返回 x，然后 x 加 1  
-- x	预递减	x 减 1，然后返回 x  
x --	后递减	返回 x，然后 x 减 1  

## 逻辑运算符
x and y  
x or y  
x xor y 异或  如果 x 和 y 有且仅有一个为 true，则返回 true  
x && y 与 如果 x 和 y 都为 true，则返回 true  
x || y  或  如果 x 和 y 至少有一个为 true，则返回 true  

三元运算符  
(expr1) ? (expr2) : (expr3) 


if...elseif....else

```
<?php
switch (n)
{
case label1:
    如果 n=label1，此处代码将执行;
    break;
case label2:
    如果 n=label2，此处代码将执行;
    break;
default:
    如果 n 既不等于 label1 也不等于 label2，此处代码将执行;
}
?>

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

$favor = "red";
switch($favor){
    case "red"://只要写条件
        echo "zhelizenmecuol";
        break; // 别忘记break不然它会全打印一遍
    case "blue":
        echo "haoba";
        break;
    default:
        echo "hapde";
        break;
}
?>
```

```
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


## 循环


    while - 只要指定的条件成立，则循环执行代码块  
    do...while - 首先执行一次代码块，然后在指定的条件成立时重复这个循环  
    for - 循环执行代码块指定的次数  
    foreach - 根据数组中每个元素来循环代码块（用来遍历数组）  
```
$x=array("Google","Runoob","Taobao");
foreach ($x as $value)
{
    echo $value . PHP_EOL;
}

$x=array(1=>"Google", 2=>"Runoob", 3=>"Taobao");
foreach ($x as $key => $value)
{
    echo "key  为 " . $key . "，对应的 value 为 ". $value . PHP_EOL;
}
```

#### 命名空间 
namespace MyProject;  
为了区别同样的函数或者类或者常量，通过命名空间区分。


## 继承  
```
class Child extends Parent {
   // 代码部分
}
```
如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。


## 访问权限

    public（公有）：公有的类成员可以在任何地方被访问。  
    protected（受保护）：受保护的类成员则可以被其自身以及其子类和父类访问。  
    private（私有）：私有的类成员则只能被其定义所在的类访问。  
    
```
// 声明一个公有的构造函数
    public function __construct() { }

    // 声明一个公有的方法
    public function MyPublic() { }`
```

## 接口：  
```
// 声明一个'iTemplate'接口
interface iTemplate
{
    public function setVariable($name, $var);
    public function getHtml($template);
}


// 实现接口
class Template implements iTemplate
```

## 表单  
```
<?php
$q = isset($_GET['q'])? htmlspecialchars($_GET['q']) : '';
if($q){
    if($q == 'RUNOOB'){
        echo '菜鸟';

    }else if($q=='GOOGLE'){
        echo "google";

    }else if ($q=="TAOBAO"){
        echo "taobao";
    }
}else{}

?>
<form acction='' method="get">
    <select name="q">
        <option value="">选择一个站点：</option>
        <option value="RUNOOB">RUNOOB</option> //VALUE一定要对
        <option value="GOOGLE">GOOGLE</option>
        <option value="TAOBAO">TAOABO</option>
    </select>
    <input type="submit" value="submit">
</form>


<?php
$q = isset($_POST['q'])? $_POST['q'] : '';
if(is_array($q)) {
    $sites = array(
            'RUNOOB' => '菜鸟教程: http://www.runoob.com',
            'GOOGLE' => 'Google 搜索: http://www.google.com',
            'TAOBAO' => '淘宝: http://www.taobao.com',
    );
    foreach($q as $val) {
        // PHP_EOL 为常量，用于换行
        echo $sites[$val] . PHP_EOL;
    }
      
} else {
?>
<form action="" method="post"> 
    <select multiple="multiple" name="q[]">
    <option value="">选择一个站点:</option>
    <option value="RUNOOB">Runoob</option>
    <option value="GOOGLE">Google</option>
    <option value="TAOBAO">Taobao</option>
    </select>
    <input type="submit" value="提交">
    </form>
<?php
}
?>



<?php
$q = isset($_GET['q'])? htmlspecialchars($_GET['q']) : '';
if($q) {
        if($q =='RUNOOB') {
                echo '菜鸟教程<br>http://www.runoob.com';
        } else if($q =='GOOGLE') {
                echo 'Google 搜索<br>http://www.google.com';
        } else if($q =='TAOBAO') {
                echo '淘宝<br>http://www.taobao.com';
        }
} else {
?><form action="" method="get"> 
    <input type="radio" name="q" value="RUNOOB" />Runoob
    <input type="radio" name="q" value="GOOGLE" />Google
    <input type="radio" name="q" value="TAOBAO" />Taobao
    <input type="submit" value="提交">
</form>
<?php
}
?>



<?php
$q = isset($_POST['q'])? $_POST['q'] : '';
if(is_array($q)) {
    $sites = array(
            'RUNOOB' => '菜鸟教程: http://www.runoob.com',
            'GOOGLE' => 'Google 搜索: http://www.google.com',
            'TAOBAO' => '淘宝: http://www.taobao.com',
    );
    foreach($q as $val) {
        // PHP_EOL 为常量，用于换行
        echo $sites[$val] . PHP_EOL;
    }
      
} else {
?><form action="" method="post"> 
    <input type="checkbox" name="q[]" value="RUNOOB"> Runoob<br> 
    <input type="checkbox" name="q[]" value="GOOGLE"> Google<br> 
    <input type="checkbox" name="q[]" value="TAOBAO"> Taobao<br>
    <input type="submit" value="提交">
</form>
<?php
}
?>

什么是 htmlspecialchars()方法?

htmlspecialchars() 函数把一些预定义的字符转换为 HTML 实体。

预定义的字符是：

    & （和号） 成为 &amp;
    " （双引号） 成为 &quot;
    ' （单引号） 成为 &#039;
    < （小于） 成为 &lt;
    > （大于） 成为 &gt;


$_SERVER["PHP_SELF"] 可以通过 htmlspecialchars() 函数来避免被利用，不被黑客攻击。  

<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">  

<?php
$nameErr = $emailErr = $genderErr = $websiteErr = '' ;
$name = $email = $gender = $commit = $website = '';

if ($_SERVER["REQUEST_METHOD"] == "POST"){
    if(empty($_POST['name'])){
        $nameErr = "名字是必需的";
    }else{
        $name = test_input($_POST["name"]);
    }
    if(empty($_POST['email'])){
        $emailErr = "邮箱是必要的";
    }else{
        $email = test_input($_POST["email"]);
    }
    if(empty($_POST['website'])){
        $website = '';
    }else{
        $website = test_input($_POST['website']);
    }
    if(empty($_POST['comment'])){
        $comment = "";
    }else{
        $comment = test_input($_POST["comment"]);
    }
    if(empty($_POST['gender'])){
        $genderErr = "必须填写性别";
    }else{
        $gender = test_input($_POST["gender"]);
    }

}
?>
<form method="post" action="<?php echo htmlspecialchars($_SERVER['PHP_SELF']);?>"> 
   名字: <input type="text" name="name">
   <span class="error">* <?php echo $nameErr;?></span>
   <br><br>
   E-mail: <input type="text" name="email">
   <span class="error">* <?php echo $emailErr;?></span>
   <br><br>
   网址: <input type="text" name="website">
   <span class="error"><?php echo $websiteErr;?></span>
   <br><br>
   备注: <textarea name="comment" rows="5" cols="40"></textarea>
   <br><br>
   性别:
   <input type="radio" name="gender" value="female">女
   <input type="radio" name="gender" value="male">男
   <span class="error">* <?php echo $genderErr;?></span>
   <br><br>
   <input type="submit" name="submit" value="Submit"> 
</form>
	
<?php
// 定义变量并默认设置为空值
$nameErr = $emailErr = $genderErr = $websiteErr = "";
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
   if (empty($_POST["name"])) {
      $nameErr = "Name is required";
      } else {
         $name = test_input($_POST["name"]);
         // 检测名字是否只包含字母跟空格
         if (!preg_match("/^[a-zA-Z ]*$/",$name)) {
         $nameErr = "只允许字母和空格"; 
         }
     }
   
   if (empty($_POST["email"])) {
      $emailErr = "Email is required";
   } else {
      $email = test_input($_POST["email"]);
      // 检测邮箱是否合法
      if (!preg_match("/([\w\-]+\@[\w\-]+\.[\w\-]+)/",$email)) {
         $emailErr = "非法邮箱格式"; 
      }
   }
     
   if (empty($_POST["website"])) {
      $website = "";
   } else {
      $website = test_input($_POST["website"]);
      // 检测 URL 地址是否合法
     if (!preg_match("/\b(?:(?:https?|ftp):\/\/|www\.)[-a-z0-9+&@#\/%?=~_|!:,.;]*[-a-z0-9+&@#\/%=~_|]/i",$website)) {
         $websiteErr = "非法的 URL 的地址"; 
      }
   }

   if (empty($_POST["comment"])) {
      $comment = "";
   } else {
      $comment = test_input($_POST["comment"]);
   }

   if (empty($_POST["gender"])) {
      $genderErr = "性别是必需的";
   } else {
      $gender = test_input($_POST["gender"]);
   }
}
?>
```

### 时间
```echo date("Y-m-d");```
```<?php include 'header.php'; ?>
fopen() 函数用于在 PHP 中打开文件。	
	
<?php
session_start();
 
if(isset($_SESSION['views']))
{
    $_SESSION['views']=$_SESSION['views']+1;
}
else
{
    $_SESSION['views']=1;
}
echo "浏览量：". $_SESSION['views'];
?>
	
mail(to,subject,message,headers,parameters) 
```
	
# 异常和错误
Try - 使用异常的函数应该位于 "try" 代码块内。如果没有触发异常，则代码将照常继续执行。但是如果异常被触发，会抛出一个异常。
Throw - 里规定如何触发异常。每一个 "throw" 必须对应至少一个 "catch"。
Catch - "catch" 代码块会捕获异常，并创建一个包含异常信息的对象

PHP 过滤器用于验证和过滤来自非安全来源的数据。
```	
<?php
$servername = "localhost";
$username = "";
$password = "";
 
// 创建连接
$conn = new mysqli($servername, $username, $password);
 
// 检测连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
} 
echo "连接成功";
?>```

	
# 文件：
引用文件有两个方法：
include（"文件名"）;和
require "文件名";
