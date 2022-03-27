### 路由规则：route提供给控制器
- 请求方式（‘请求路由’，匿名函数或控制器响应)
- 必选参数{参数}，可选参数{参数?}
- match(匹配)指定多个路由,any(任何)
```
Route::get('/',function(){
  return view('welcome');});
  
Route::match(['get','post'],'/',function(){
  return 'hello world';})->name('名字'); //起名
```
#### 路由群组：
 ```
 Route::group[['prefic'=>'admin']function(){
        Route::get('ri',fucntion(){})}]
 ```

## 控制器
- 创建指定目录的控制器（分目录管理）：php artisan make:controller Admin/UserController


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

## 数据库操作：
- 执行任意的 insert,update,delete 语句(形象记录的语句使用statement语法)
> DB::statement("insert into member values(null,'')");
- 执行任意的select语句(不影响记录的语句使用select语句)
> DB::select("select * from member");
```
create table member(
  id int primary key auto increment,
  name varchar(32) not null,)
  
 SB_HOST:locahost -> 不写本地域名是因为写域名默认远程登陆
 
 DB:: -> 在app.php里定义了别名DB,不用写太长的空间方法
```

### 增加
- insert():添加一条或多条，返回布尔类型
- insertGetId():添加一条，返回自增的id
```
public function add(){
    $db = DB::table('users');
    //定义变量，可以多次反复使用
    $result = $db -> insert([
        [
            'name' => '马冬梅',
            'email' => 'madongmei@gmial.com',
            'password' => 'qwerty',
        ],
        [
            'name' => '马春',
            'email' => 'machun@gmial.com',
            'password' => 'qwerty',
        ],
    ]);
    dd($result);

    $result = $db -> insertGetId([
            'name' => '马秋梅',
            'email' => 'macqiumei@gmial.com',
            'password' => 'qwerty',
    ]);
    dd($result);
}
```

### 修改
- where（字段，运算符，值)
```
public function update(){
    $db = DB::table('users');

    $result = $db -> where('id',14) -> update([
        'name' => '张三丰',
    ]);
    dd($result);//返回收到影响的行数
}

increment 和 decrement
DB::table('users') -> increament('votes') 每次加1
DB::table('users') -> increament('votes',5) 每次加5
DB::table('users') -> decreament('votes') 每次减1
DB::table('users') -> decreament('votes',5)每次减5
```
### 查询
```
public function select(){
    $db = DB::table('users');
    $data = $db -> where('id','>',3) -> get();

    foreach ($data as $key => $value){
        echo "id是：{$value -> id},名字是: {$value -> name}.<br/>";
    }//get查询的结果每一行记录的是对象,value是对象不是数组，所以获取遍历数组用的是箭头
}

//->where()->where() 并且关系语法
//->where()->orWhere() 或者关系

输出users表里所有的用户名
public function select(){
    $db = DB::table('users');
    $data = $db -> get();
    foreach ($data as $datas){
        echo"{$datas -> name}<br/>";}
    }
//返回特定用户的某个字段值
$data = $db -> where('id',1)->value('email');
return $data;
//返回特定用户的多个字段值

//排序
$db = DB::table('users')
    ->orderBy('id', 'desc')//倒序
    ->get();
return $db;

//分页
$db = DB::table('users')
    ->limit(2)
    ->offset(1)
    ->get();
return $db;
```


### 删除
- 修改代替删除
- 物理删除（本质就是删除），逻辑删除（本质是修改）
- 逻辑删除本质是不把信息显示给用户

```
$db = DB::table("users");
$result = $db -> where('id','1')->delete();
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
