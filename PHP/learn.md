# PHP 7.4
PHP（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。
PHP 脚本在服务器上执行。  
注释： // /* */

## 变量
以 $ 符号开始，区分大小写  
变量有：local，global（函数外部的变量），static，parameter  
如果要在一个函数中访问一个全局变量，需要使用 global 关键字  
 ```
 <?php
$x = 5;
$y = 6;
function myTest()
{
	global $x,$y; // 全局变量
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
```
