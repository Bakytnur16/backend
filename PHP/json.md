#JSON
- JavaScript Object Notation（JavaScript 对象标记法）。
- JSON 是一种存储和交换数据的语法。
- JSON 属于文本，并且我们能够把任何 JavaScript 对象转换为 JSON，然后将 JSON 发送到服务器

#### 在 JSON 中，值必须是以下数据类型之一：
- 字符串 字符串值必须由双引号编写
- 数字
- 对象（JSON 对象）
```{
"employee":{ "name":"Bill Gates", "age":62, "city":"Seattle" }
}
```
- 数组
```
{
"employees":[ "Bill", "Steve", "David" ]
}

```
- 布尔
- null

- JSON 中不允许日期对象。如果您需要包含日期，请写为字符串。之后您可以将其转换回日期对象：
- 不允许函数【转换字符串】，使用 eval() 将它们转换回函数。
JSON.stringify() 将它转换为字符串。

### php和json
- 通过使用 PHP 函数 json_encode()，PHP 中的对象可转换为 JSON
- 使用 JSON.stringify() 将 JavaScript 对象转换为 JSON
```
<?php
$myObj->name = "Bill Gates";
$myObj->age = 62;
$myObj->city = "Seattle";

$myJSON = json_encode($myObj);

echo $myJSON;
?>

<?php
header("Content-Type: application/json; charset=UTF-8");
$obj =  json_decode($_POST["x"], false);
  
$conn = new mysqli("myServer", "myUser", "myPassword", "Northwind");
$result = $conn->query("SELECT name FROM ".$obj->$table." LIMIT ".$obj->$limit);
$outp = array();
$outp = $result->fetch_all(MYSQLI_ASSOC);

echo json_encode($outp);
?>
```
