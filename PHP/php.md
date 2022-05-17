php课程：https://edu.aliyun.com/course/509?utm_content=m_1000015030
https://zhuanlan.zhihu.com/p/24035779

# PHP
- PHP：Hypertext Preprocessor 超文本预处理器  
- PHP 脚本在服务器上执行。    
- 注释： // /* */[shift+ctrl+/]  
- PHP是动态语言，是一种非常弱的类型语言，在程序运行时，可以动态的改变变量的类型。  

#### 打印
- 单引号和双引号有区别  
1. echo - 可以输出一个或多个字符串(echo 输出的速度比 print 快， echo 没有返回值，print有返回值1)  
> echo "hello"," world"," welocome";
> echo "his name is $username!";//双引号可以解析变量和转义字符
2. print - 只允许输出一个字符串，返回值总为 1  
> print "在 $txt2 学习 PHP ";
> print "我车的品牌是 {$cars[0]}";

4. printf("a weekend have %d days",7)  
>> %d ：整数，显示为有符号十进制数  
>> %f ：浮点数，显示为浮点数  
>> %s ： 字符串，显示为字符串
>> 

## 变量
- 以 $ 符号开始，区分大小写  
#### 变量类型：
- local 局部作用域
- global（函数外部的变量：在函数被内部引用需要： global $name;
> PHP 将所有全局变量存储在一个名为 **$GLOBALS[index]** 的数组中
```
$x = 5;
$y = 6;
function my(){
    $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
    //这个数组可以在函数内部访问，也可以直接用来更新全局变量
}
my();
echo $y;
```
- static（静止不被删除): 当一个函数完成时，它的所有变量通常都会被删除,为了保留局部变量
```
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
```
- parameter（函数参数作用域）：fucntion(parameter){}  

#### 判断变量：
- gettype: echo gettype ( $sum );  
- settype: echo settype ( $sum, "float" );  
双引号可以解析变量和转义字符
 ```
Gettype() 获取变量的类型
Settype() 设置变量的类型 Bool(1:true 0:false(or ’’))
Isset()	用来判断一个变量是否存在 Bool isset($a);
Unset()	释放给定的变量	Void
Empty()	检测一个变量的值是否为空 Bool
is_int() is_integer()	检测变量是否是整数 Bool
Is_string()	检测变量是否是字符串 bool
Is_numeric	检测变量是否为数字或数字字符串	bool
Is_null	检测变量是否为 NULL	bool
Intval()	获取变量的整数值 int
 
<?php
$sum = 100;
$total = ( string ) $sum;
echo gettype ( $sum );//string
?>
```
#### 超级变量：
```
    $GLOBALS
    $_SERVER: $_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。  
    $_REQUEST: PHP $_REQUEST 用于收集HTML表单提交的数据。  

<?php 
$name = $_REQUEST['fname']; 
echo $name; 
?>

$GLOBALS	所有全局变量数组
$_SERVER	服务器环境变量数组
$_GET	通过GET方式传递给该脚本的变量数组
$_POST	通过POST方式传递给该脚本的变量数组
$_COOKIE	COOKIE变量数组
$_FILES	与文件上传相关的变量数组
$_ENV	环境变量数组
$_REQUEST	所用用户输入的变量数组
$_SESSION	会话变量数组
```

#### EOF(heredoc)
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
 ```
 $x = 10.6;
var_dump($x);
返回： double(10.6)
 ```
### String（字符串）
- 并置运算符 (.) 用于把两个字符串值连接起来。
- php弱类型语言，所以'7'+$x = 12是成立的  // echo '7' . $a; 75
```
$txt1="hello world";
$txt2="what a nice day";
echo $txt1 ." ". $txt2;
hello world what a nice day

strlen() 字符串长度  
strpos() 函数用于在字符串内查找一个字符或一段指定的文本,返回第一个匹配的字符位置 
chop()函数移除字符串后面多余的空白，包括新行。  
ltrim()函数移除字符串起始处多余空白。  
rtrim()函数移除字符串后面多余的空白，包括新行，此函数是chop()的别名。  
trim()函数移除字符串两边多余的空白。  
nl2br()函数将字符串作为输入参数    
strtoupper()函数将字符串转换为大写   
strtolower()函数将字符串转换成小写  
ucfirst()函数将第一个字母转换为大写  
ucwords()函数将每个单词第一个字母转换为大写  
substr()允许我们访问一个字符串给定起点和终点的子字符串。
strtok()函数一次只从字符串取出一些片段（称为令牌）。对于一次从字符串中取出一个  
<?php echo substr("abcdef", 1, 3); ?>  
str_split()返回一个数组，其中各数组元素分别是字符串参数中的一个字符串。
<?php print_r(str_split('This is a Teacher!')); ?>
substr_count()返回一个字符串在另一个字符串中出现的次数。  
可以使用函数strlen()来检查字符串的长度  
替换字符串：str_replace()、str_ireplace()、substr_replace()  
```
 
 #### 正则表达式
 preg_grep()函数搜索数组中的所有元素、preg_match()preg_match()函数在字符串中搜索模式、preg_match_all()函数在字符串中匹配模式的所有出现、preg_quote()在每个对于正则表达式语法而言有特殊含义的字符前插入一个反斜线、preg_replace()函数搜索到所有匹配，然后替换成想要的字符串返回出来。、preg_replace_callback()和 preg_split()用来分割不同的元素。  


### Integer（整型）
- 整型可以用三种格式来指定：十进制， 十六进制（ 以 0x 为前缀）或八进制（前缀为 0） 
``` 
Rand()函数是libc中定义的一个随机函数的简单包装器。$a = rand(0,10);  
Mt_rand()函数是一个很好的代替实现. $b = mt_rand(0,10);  
Abs()	取绝对值  
Floor()	舍去法取整  
Ceil()	进一法取整  
Round()	四舍五入  
Min()	求最小值或数组中最小值  
Max()	求最大值或数组中最大值  
```

### Float（浮点型）可以有指数
double

### Boolean（布尔型）
> $x=true; $y=false;

### Array（数组）
- 可以用短数组语法 [] 替代 array() 。

```
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
$cars[0]="Volvo";    
echo count($cars);  
count() 函数用于返回数组的长度

$cars = array("voloe","boolm","toyota");
echo $cars[0]; 

//遍历
for($x=0;$x<count($cars);$x++){
    echo $cars[$x];
    echo "<br>";
}

// 长度是3
$cars[0]="Volvo";
$cars[1]="BMW";
$cars[2]="Toyota"; 
```
##### 数组排列：
```
    sort() - 对数组进行升序排列
    rsort() - 对数组进行降序排列
    asort() - 根据关联数组的值，对数组进行升序排列
    ksort() - 根据关联数组的键，对数组进行升序排列
    arsort() - 根据关联数组的值，对数组进行降序排列
    krsort() - 根据关联数组的键，对数组进行降序排列
```

#### 关联数组（像字典）  
```
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43"; 

$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
foreach($age as $x => $y){
    echo "key=" .$x.", value=" . $y;
} 
 ```
 ##### 数组运算符
 ```
 x + y	集合	x 和 y 的集合
x == y	相等	如果 x 和 y 具有相同的键/值对，则返回 true
x === y	恒等	如果 x 和 y 具有相同的键/值对，且顺序相同类型相同，则返回 true
x != y	不相等	如果 x 不等于 y，则返回 true
x <> y	不相等	如果 x 不等于 y，则返回 true
x !== y	不恒等	如果 x 不等于 y，则返回 true
 ```
 

#### Object（对象） laravel 重点
```
$this-> color = $color
// PHP关键字this指向当前对象实例的指针，不指向任何其他对象或类。
```
#### NULL（空值）
> $x=null;  

#### 资源类型 get_resource_type()
```
$c = mysql_connect();
echo get_resource_type($c)."\n";
// 打印：mysql link
```

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
- 一旦被定义后，就不能更改
- bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
> name：必选参数，常量名称，即标志符。  
> value：必选参数，常量的值。  
> case_insensitive ：可选参数，如果设置为 TRUE，该常量则大小写不敏感。默认是大小写敏感的。  
- PHP 向它运行的任何脚本提供了大量的预定义常量。  
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
### 运算符

++ x	预递增	x 加 1，然后返回 x  
x ++	后递增	返回 x，然后 x 加 1  
-- x	预递减	x 减 1，然后返回 x  
x --	后递减	返回 x，然后 x 减 1  

#### 逻辑运算符
x and y  
x or y  
x xor y 异或  如果 x 和 y 有且仅有一个为 true，则返回 true  
x && y 与 如果 x 和 y 都为 true，则返回 true  
x || y  或  如果 x 和 y 至少有一个为 true，则返回 true  
- 运算符”and”和”or”比&&和||的优先级要低  

### 三元运算符  
(expr1) ? (expr2) : (expr3) 
条件表达式?表达式1:表达式2。
```
<?php
$a = 100;
echo $a > 60 ? 'success':'fail';
?>

if...elseif....else


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

##### 组合比较符
```
$c = $a <=> $b;
如果 $a > $b, 则 $c 的值为 1。
如果 $a == $b, 则 $c 的值为 0。
如果 $a < $b, 则 $c 的值为 -1。
```

## 循环
- while - 只要指定的条件成立，则循环执行代码块  
- do...while - 循环先执行一次再去判断条件，也就是说不管满不满足条件，都会先执行一次，执行次数最少1次 
- for - 循环执行代码块指定的次数  
- foreach - 根据数组中每个元素来循环代码块（用来遍历数组）  

```
$i=1;
while($i<=5)
{
    echo "The number is " . $i . "<br>";
    $i++;
}

do
{
    $i++;
    echo "The number is " . $i . "<br>";
}
while ($i<=5);

for (初始值; 条件; 增量) //for ($i=1; $i<=5; $i++)
{
    要执行的代码;
}

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

<?php
$num = range(0,11);
foreach( $num as $i){
    echo $i . '.';
}
?>
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
标头(header) 是服务器以HTTP 协议传HTML 资料到浏览器前所送出的字符串，在
标头与HTML 文件之间尚需空一行分隔。

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
<form action='' method="get">
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
戳（Unix Timestamp）。Unix 时间戳(Unix timestamp)，或称Unix 时间(Unix time)、POSIX 时间(POSIX time)  
```echo date("Y-m-d");```
checkdate()函数能够很好地验证日期，提供的日期如果有效，则返回true  
date()函数返回根据预定义指令格式化时间和日期的字符串形式  
gettimeofday()函数返回与当前时间有关的元素所组成的一个关联数组   
getdate()函数接受一个时间戳，并返回一个由其各部分组成的关联数组。  
time()函数可以获取当前的时间戳，并且可以通过设置时间戳的值。   
mktime()函数可以生成给定日期时间的时间戳。  
strtotime()将人可读的日期转换为Unix 时间戳。  
getlastmod()可以得到当前文件最后修改时间的时间戳   
putenv()函数可以设置当前的默认时区。  
date_default_timezone_set()可以设置当前的默认时区。  
localtime()函数可以取得本地时间数据，然后返回一个数组。  

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

	
#文件：
引用文件有两个方法：
include（"文件名"）;和
require "文件名";
Include后面如果还有其他代码，当调用include出错时，后面的代码还会继续执行，但是require则不会。
Include在调用一个不存在的文件时，会给出警告，但是会继续执行后面的代码。

basename — 返回路径中的文件名部分  
dirname — 返回路径中的目录部分  
pathinfo — 返回文件路径的信息  
realpath — 返回规范化的绝对路径名  
filesize — 取得文件大小  
disk_free_space — 返回目录中的可用空间  
disk_total_space — 返回一个目录的磁盘总大小  
fileatime — 取得文件的上次访问时间  
filectime — 取得文件的 inode 修改时间  
filemtime — 取得文件修改时间  
fopen — 打开文件或者 URL  
fclose — 关闭一个已打开的文件指针  
fwrite — 写入文件（可安全用于二进制文件） 
file_exists — 检查文件或目录是否存在  
feof — 测试文件指针是否到了文件结束的位置  
r 只读 r+ 读写指针在开头 w写 w+ 读写指针在末尾
```
<?php
$fp = fopen('file1.txt','w');
$outStr = "my name is anllin,\r\nmy age is 29.";
fwrite($fp,$outStr,strlen($outStr));
fclose($fp);
?>
```
规范：
namespace Vendor\Model;  
类的常量中所有字母都必须大写，词间以下划线分隔。   
类的属性命名可以遵循 大写开头的驼峰式，小写开头的驼峰式，下划线分隔式   
方法名称必须符合 camelCase() 式的小写开头驼峰命名规范。  

所有PHP文件必须使用Unix LF (linefeed)作为行的结束符。  
所有PHP文件必须以一个空白行作为结束。  
纯PHP代码文件必须省略最后的 ?> 结束标签。  
每行不应该多于80个字符  
代码必须使用4个空格符的缩进，一定不能用 tab键 。  
不要使用下划线作为前缀，来区分属性是 protected 或 private。  
