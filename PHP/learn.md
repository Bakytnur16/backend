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
常量：  
__LINE__
```
<?php
echo '这是第 " '  . __LINE__ . ' " 行'; //这是第 “ 2 ” 行
?>
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
