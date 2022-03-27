laravel:增删改查+循环+判断  
- 注意： LTS(长期支持）
- composer是php的appstore: 
> composer global require laravel/installer - laravel new blog   
> composer create-project laravel/laravel=5.4.* edu --prefer-dist ./--prefer-dist 优先下载压缩包 
- composer require barryvdh/laravel-ide-helper  
> php artisan ide-helper:generate 为Facades生成注释  
> php artisan ide-helper:models 为数据模型生成注释
> php artisan ide-helper:meta 生成phpstorm meta file
- 先读取.env在执行config配置命令，所以要在env里先设置
- php artisan serve 修改了配置文件需要重启    
- artisan生成，必须在项目目录下执行。
bixuan/ 和bixuan是两个url       

## 路由：接收HTTP请求的路径/ route提供给控制器  
- 请求方式（‘请求路由’，匿名函数或控制器响应)
- 必选参数{参数}，可选参数{参数?}
- match(匹配)指定多个路由,any(任何）：
```
Route::get('/',function(){
  return view('welcome');});
  
Route::match(['get','post'],'/',function(){
  return 'hello world';})->name('名字'); //起名

Route::get('/', function () {
    return 'hello world';
});

Route::get('index/{id}',function($id){ //参数，动态变量
    return 'hello world'.$id;
})

Route::get()
Route::post()
Route::any()接受任何请求；
Route::match()接受特定的提交方式：

Route::match(['get','post'],'index',function() //必须有三个参数
{
  return 'hello world';
});

```
### 路由命名
- 给路由命名可以生成url地址或进行重定向
- URL是URI的子集
```
    public function url(){
        $url = route('task.index');
        return $url;
    }
Route::get('task/url',[TaskController::class, 'url'])
    ->name('task.index');//控制器的名称+方法 //第三个函数false取消域名
```

#### 分组
```
Route::prefix('api')->get('task',[TaskController::class,'index']);

Route::group(['prefix'=>'api'],function(){
    Route::get('task/url',[TaskController::class, 'url'])->name('task.index');
    Route::get('task',[TaskController::class, 'index']);
});
//推荐
Route::prefix('api')->group(function(){
    Route::get('task/url',[TaskController::class, 'url'])->name('task.index');
    Route::get('task',[TaskController::class, 'index']);
});
```

### 路由跳转
- http协议
```
Route::redirect('index','task',301);//跳转，302临时跳转,301永久跳转
Route::permanentRedirect('index','task');//直接设置301
```

### 回退路由
- 使用回退，让不存在的路由自动跳转到你指定的页面去
- 必须把回退路由放到所有路由的底部
```
在view里创建404.blade.php 文件：做美观一点就行了
Route::fallback(function(){
//    return redirect(to:'/');
    return view('404');
});
//获取路由信息
Route::get('task',function(){
    dump(Route::current());
});
```

#### 批量资源路由
```
Route::resource([
    'blogs'=> 'BlogController'
    ]);
    
public function edit($id){
    return route('blogs.edit',['blog'=>10]);edit默认情况下是blog,所以写id不会被识别。
    
Route::resource('blogs',BlogController::class)
    ->only('index','show');//只生成这两个
    
Route::except('blogs',BlogController::class)
    ->except('index','show');//除这两个之外
```
#### api资源路由
```
Route::apiResources([
    'blogs' => 'BlogController'
]);
Route:apiresource('blog',BlogController);

php artisan route:list目前路由注册的列表
php artisan make:controller CommentController --api

http://127.0.0.1:8000/blogs/10/comments/20/edit

public function edit($blog_id, $comment_id)
{
    return '编辑博文下的评论，博文id： '.$blog_id.'评论id： '.$comment_id;
}
```
#### 资源路由嵌套
- Route::resource('blogs.comments',\App\Http\Controllers\CommentController::class);
- 一般情况下id是独立的，所以不需要父id和子id同时存在；
为了优化资源现套，通过->shallow()实现浅层套现方法
Route::resource('blogs.comments',\App\Http\Controllers\CommentController::class)->shadow();

# 控制器：接受http请求
- MVC里的C
- 用大头方法命名
- 创建控制器： php artisan make:controller TaskController
- 创建指定目录的控制器（分目录管理）：php artisan make:controller Admin/UserController
```
namespace App\Http\Controllers;
Route::get('/task',[TaskController::class, 'index']);//参数二：控制器@方法名
Route::get('/task/read/{id}',[TaskController::class, 'read']); //参数设置
Route::get('task','App\Http\Controllers\TaskController@index');

//正则表达限制动态参数

Route::get('/task/read/{id}',[TaskController::class, 'read'])
    ->where('id',[0-9]+'); 

Route::get('/task/read/{id}',[TaskController::class, 'read'])
    ->where(['id'=>[0-9]+','name'=>'[a-z]+']); 
    
//正则全局路径设定：Providers/routeserviceprovider.php里

boot(){
    Route::pattern('id','[0-9]+');
    pattern::boot();
    }
    
//在某个路由解除全局正则：

Route::get('/task/read/{id}',[TaskController::class, 'read'])->where('id','.*');
```
## 单行控制器：__invoke() 固定的方法
php artisan make:controller OneController --invokable //声明
- 只是语义但行为，没限制不能用其他方法
- 路径里不用定义方法，直接访问，只处理一个功能
```
use App\Http\Controllers\OneController;
Route::get('one',OneController::class);
    public function __invoke()
    {
        return 'break';
    }
    
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

# 视图路由:view
- MVC中的view
- 视图路径（三个参数）：url，名称[view文件名称]，参数
- 文件在在resource/views/xxx.blade.php 形式创建

- html快速创建： 输入'html:5 +TAB键'

```
Route::get('/task',[TaskController::class, 'index'])
Route::view('task','task',['id'=>10]);
或者
控制器里输出view
index函数里：return view('task',['id'=>10]);

Route::get('index',function(){
    $time = strtotime(now());
    return View('index',compact('time'));
});

<a>{{date('Y-m-d',$time)}}</a>

use Illuminate\Support\Facades\DB;

public function select(){
    $lists =  DB::table('users')
        ->where('id','2')
        ->get();
    return View('index',compact('lists'));
}

Route::get('select',[TaskController::class,'select']);
```

#### 中间件

#### 子域名 domain
```
Route::domain('127.0.0.1')->group(function(){
    Route::get('task/url',[TaskController::class, 'url'])->name('task.index');
    Route::get('task',[TaskController::class, 'index']);
});
```
#### 命名空间
- 比如在conrtoller创建一个admin文件夹，再加一个controller
```
Route::namespace('Admin')->group(function(){
//只能访问控制器里的，或者不在控制器里的
    Route::get('task/url',[TaskController::class, 'url'])->name('task.index');
    Route::get('task',[TaskController::class, 'index']);
});
```
#### name
```
可以嵌套

Route::name('admin.')->group(function() {
    Route::get('task/url', [TaskController::class, 'url'])->name('task.index');
    Route::name('abc.')->group(){
        Route::get('task',[TaskController::class, 'index']);
});
```

## 响应设置
- JSON格式
- 处理完业务会返回一个发送到浏览器的响应，return
- responce（）可以做更多的相应设置和状态码
```
字符串-字符串输出；数组-json格式
return response()->json([1,2,3]);

跳转首页
return redirect()->to('/');
return redirect('task');
return redirect()->route('task.index');
return back(); 返回上一页
return redirect()->away('http://www.baidu.com');
```

### 资源型控制器
#### 单个批量资源路由
- 平常的增删改查
```
Route::resource('blogs',BlogControler::cass)
```
- php artisan make:controller UserController --resource
> 会生成7个方法
![serven-method](https://user-images.githubusercontent.com/64322636/156432163-a31bf0cf-fb3c-49a6-9e58-e79fe38e8875.png)

## 数据库操作：
- 执行任意的 insert,update,delete 语句(形象记录的语句使用statement语法)
> DB::statement("insert into member values(null,'')");
- 执行任意的select语句(不影响记录的语句使用select语句)
> DB::select("select * from member");
- 数据库配置要记得备份
> php artisan make:migration create_hd_table --create=hd
> php artisan migrate
> php artisan migrate:refresh
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

### 数据库配置
- ELOQUENT  ORM 关系型对象映射器来操作数据库；
- 数据库配置在config/database.php
- 本地配置在.env
```
DB_CONNECTION=pgsql
DB_HOST=[localhost]
DB_PORT=
DB_DATABASE=demodb
DB_USERNAME=postgres
DB_PASSWORD=

DataController:
use Illuminate\Support\Facades\DB;
$user = DB::table('admin')->find(id:1);
return Response()->json($user);

```
数据库有专有DB，可以查询和构造器查询


#### 报错：could not driver
- window配置：
> php.ini：
> extension=php_pdo_pgsql.dll  
> extension=php_pgsql.dll  
- 重启apache：d:/apache/apache24/bin:httpd –k restart

### 构造器查询
#### 查询
-table()引入相应的表
- first()获取第一个数据
- 获取第一个数据的email字段
- find(id)通过id指定一条数据
- plunck(字段名'email','username')获取所有数据单列值得集合；
```
$users = DB::table('admin')->get();
$users = DB::table('admin')->first();
$users = DB::table('admin')->value('email');
$users = DB::table('admin')->find(20);
$users = DB::table('admin')->pluck('username','id');//id作为username的key
return $users;
```
#### 分块处理
```
DB::table('admin')->orderBy('id')->chunk(3, function($users){ //前三条
    foreach($users as $user){
        echo $user->name;}
    echo '---<br>';
});
```
#### 聚合查询
- 聚合函数返回结果，必须放到最后，像orderby和where应该放到句中
```
return DB::table('admin')->count(); //有多少个
return DB::table('admin')->avg('price');//价格的平均值
return DB::table('admin')->where('id',10)->get();
return [DB::table('admin')->where('id',10)->exists()]; 不存在返回false，存在返回true
return [DB::table('admin')->where('id',10)->exists()]; //正好相反，没有就回true
```

#### select(query)
```
$user  = DB::table('admin')->select('name as account')->get();
[{"account":"s"},{"account":"b"},{"account":"b"},{"account":"n"}]
[{"name":"s"},{"name":"b"},{"name":"b"},{"name":"n"}]选择addSelect
//附加
$base = $user->addSelect('gender')->get();
$users = DB::table('admin')->select(DB::raw('COUNT(*) AS id'))->get();
return $users;
//查询男的和女的
$users = DB::table('admin')
    ->groupBy('gander')
    ->select(DB::raw('COUNT(*) AS count, gender'))->get(); //raw实现内部原生
return $users;

$users = DB::table('admin')
    ->groupBy('gander')
    ->havingRaw('count>4')
    ->selectRaw('COUNT(*) AS count, gender')->get();
return $users;
//havingRaw 分组之后再进行筛选
```

#### where
- 条件查询
```
$users = DB::table('admin')->where('id','>=',19)->get(); //等于就不用写operator
$users = DB::table('admin')->where('name','like','%xiao%')->get();
//条件多就用数组
$users = DB::table('admin')->where([
    'gender' => '男',
    'status' => 1
    ])->get();
    
$users = DB::table('admin')->where([
    ['gender'，'>=', 90],
    ['price','=','男‘]
    ])->get();
orWhere()实现两个或以上的or条件查询 或者
$users = DB::table('admin')->where('price','>=',95）
                           ->orwhere(function($query){
                                $query->where('gender','女')->where('username','like',%xiao%})
                                ->toSql();

//whereBetween: orWhereBetween/ whereNotBetween/ orWhereNotBetween
$users = DB::table('admin')->whereBetween('price',[60, 90])->toSql();

//whereIn ： orWhereIn/ whereNotIn/ orWhereNotIn
$user  = DB::table('admin')->whereIn('id',[20,30,50,90])->get(); //在这个数据集里的

//whereNull()查询null记录
$user  = DB::table('admin')->whereNull('uid')->get();

//whereDate()
$user  = DB::table('admin')->whereDate('create_time','>','2018-12-11')->get();
```

#### 排序分组
- whereColumn() 实现两个字段相等的查询结果
- orderBy() 实现desc或者asc排序
- latest() 时间倒序
- inRandomOrder()随机排序


#### join
```
$users = DB::table('users')
    ->join('book', 'books.user_id','user.id')
    ->tosql();
$users = DB::table('users')
    ->select('username','email')
    ->crossJoin('books')
    ->get();
    
$users = DB::table('users')->join('books',function($join){
    $join->on('books.user_id','=','users')->where('users.id',19);
})->sql();

$query = DB::table('books')->selectRaw('user_id,title');
$users = DB::table('users')->joinSub($query,'books',function($join){
})->tosql();
$users = DB::table('users')
    ->select('username','email')
    ->distinct() //取消重复
    ->get();
//支持orOn 连缀
```

### 构造器的增删改
```
//insertOrIgnore 新增不报错,屏蔽
//insertGetId 新增获取id
$query = DB::table('users')->insert([
    [//批量新增
    'username'=> '李白',
    'password' => '123456',
    'email' => 'shuak@gmail.com']);
    ],[
    'username'=> '李白',
    'password' => '123456',
    'email' => 'shuak@gmail.com'
    ]
]);

//update 
DB:table('users')-> where('id',311)
    ->update([
        'username' => '李红',
        'email' => 'ligong@gmail.com']);
        
//有数据就更改数据，没有就添加
DB::table('users')->updateOrInsert(
    ['id'=>301],
    [
    'username' => '里黑',
    'passwrd' => '654321',
    'email' => '3134@gmail.com',
    'details' => '321',]
);

DB::table('users')->where('id',19)
    ->update()[
        'list->uid' => 1010
    ];
// 自增increment(),自减decrement()

删除一条数据
DB::table('users')->where('id',307)->delete();

清空
DB::table('users')->delete();
DB::table('users')->truncate();
```

# 模型
### model
php artisan make:model Http/Models/User
- 模型编码要求数据库是复数s[es,ies]
- return Str::plural('user')返回字符串复数
- protected $keyType = 'string';
- 模式下可以更改数据库

虚拟主机


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
```
### 上传文件：
- 上传错误状态码的取值是0-7，没有5，0表示成功
