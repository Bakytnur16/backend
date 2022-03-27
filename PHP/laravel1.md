## Facades:
- 新版本启弃用了input接口
- 接受用户输入的类：Illuminate\Support\Dacades\Input
- Facades：门面的思想，门面介于一个类的实例化和没有实例化中间的状态。
```
可以获取get和post
Input::all() 
Input::get()
Input::only([]) 获取指定
Input::expect([]) 取反
Input::has('name') 判断某个输入是否存在
```

## 视图
- blade.php文件后缀
- 视图文件分目录，用.表示：return view('home.index');
- 视图文件优先运行blade.php，没有的情况下运行php，两种格式的文件都支持
- view(模板文件，数组) 或者 view(模板文件)->with(数组)
- smarty模板引擎存在"|",用于在视图中解释变量（使用函数去处理变量）
- 视图引入css文件：
>     <link rel="stylesheet" type="text/css" href="/css/app.css">
>     <link rel="stylesheet" type="text/css" href="{{asset('css')}}/app.css">
```
public function index(){
    $date = date('Y-m-d H:i:s',time());
    $day = 'Sunday';
    return view('test.welcome',['date'=>$date,'day'=> $day]);
或者：
    return view('test.welcome',compact('date','day')); //php内置函数compact(变量1，变量2）
}

<h1>现在是{{$date}},今天是{{$day}}</h1>

//get查询到的结果集中每一条记录其实是一个对象，因此在循环具体字段的时候需要注意使用对象调用属性的方式才可以获取数据   
id&emsp;&emsp;name&emsp;&emsp;email&emsp;&emsp;password<br/>
@foreach($db as $data){
    {{$data -> id}}&emsp;&emsp;{{$data -> name}}&emsp;&emsp;{{$data -> email}}<br/>
}
@endforeach

@if($data < 10)
less 10
@elseif($date == 10)
equl to 10
@else
more than 10
@endif
```
#### CSRF 跨站请求伪造
- laravel自动为每一个用户session生成一个CSRF Token,Token可用于验证登录用户和发送请求是否是同一个人
- @csrf（以前可以使用：csrf_field() ->代表整个的input隐藏域）
- 在异步提交表单的情况下必须使用csrf_token()
> <input type="hidden" name="_token" value="{{csrf_token()}}">
- csrf白名单: 在middleware/VerifyCsrfToken.php里设置,允许scrf排除例外路由
> protected $expect = ['home/test/test7',];
表单请求:
public function form(){
    return view('form');
}

<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          contesnt="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>提交</title>
</head>
<body>
<form action="/task/getform" method="post">
    <input typw="hidden" name="_token" value="{{csrf_token()}}">;
    <button type="ssubmit">提交</button>
</form>
</body>
</html>

Route::get('/task/form',[TaskController::class,'form']);
Route::post('task/getform',function(){
    return 123;});
```
419:为了避免跨站请求伪造攻击，框架提供了CSRF令牌保护，请求验证
```<input typw="hidden" name="_token" value="{{csrf_token()}}">;```
- 快捷方法：@csrf 

## 模型
- AR(ActiveRecored)模式下必须要实例化模型（不常用）
```
$team = new Team();
$team -> name = 'helloworld';
$team ->save();
```
- 每张数据表对应一个与该表进行交互的"model模型"(映射)
- Member.model在不指定table属性的情况下，会默认去找members表
- 在主键字段不是id的情况下，模型里定义主键属性$PrimaryKey
- 如果数据表里不需要$timestamps属性,在model里设置flase
- request 语法：
```
$request->all()
$request->input('name');
$request->only(['name1','name2']);
$request->except(['name1','name2'...])
$request->has('name')
$request->get('name')

$user = new User();
$user -> create($request -> all());
 or
return User::create($request -> all());

return User::find(4);

//all不支持其他查询方法，get可以在前面添加select之类的方法
get([列1,列2])
select(列1，列2)->get()
all([列1，列2])

$result = Member::where('id','7') -> update([
      'age' => '82']);
```

### 自动验证
- 后端也需要设置认证，黑客可以绕过前端验证，直接黑进服务器
```
public function store(Request $request){
  $this -> validate(数据对象,[验证规则]);
  $this- 就是当前类
requred - 不为空
max:225 最长255个字符
min:1最少字符
email:验证邮箱合法
confirmed 验证两个字段是否相同
password_confirmation
integer:判断整型
ip:验证字段是否是ip地址
numeric:验证字段是数值
max:value验证字段必须小于等于最大值
string: 是否是字符串
unique:字段唯一
| 并列
不并列

```
