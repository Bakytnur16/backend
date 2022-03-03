laravel

## 路由：接收HTTP请求的路径
- web.app

```
Route::get('/', function () {
    return 'hello world';
});

Route::get('index/{id}',function($id){ //参数，动态变量
    return 'hello world'.$id;
});

Route::get()
Route::post()
Route::any()接受任何请求；
Route::match()接受特定的提交方式：

Route::match(['get','post'],'index',function() //必须有三个参数
{
  return 'hello world';
});

```
## 控制器：接受http请求
- MVC里的C
- 创建控制器： php artisan make:controller TaskController
```
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
#### 路由跳转
- http协议
```
Route::redirect('index','task',301);//跳转，302临时跳转,301永久跳转
Route::permanentRedirect('index','task');//直接设置301
```

## 视图路由:view
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
```

## 路由命名
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

# api资源路由
Route::apiResources([
    'blogs' => 'BlogController'
]);
Route:apiresource('blog',BlogController);
```
php artisan route:list目前路由注册的列表
php artisan make:controller CommentController --api

```
http://127.0.0.1:8000/blogs/10/comments/20/edit

public function edit($blog_id, $comment_id)
{
    return '编辑博文下的评论，博文id： '.$blog_id.'评论id： '.$comment_id;
}

//资源路由嵌套
Route::resource('blogs.comments',\App\Http\Controllers\CommentController::class);

一般情况下id是独立的，所以不需要父id和子id同时存在；
为了优化资源现套，通过->shallow()实现浅层套现方法
Route::resource('blogs.comments',\App\Http\Controllers\CommentController::class)->shadow();
```
```
表单请求：
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

#### 数据库配置
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

### model
php artisan make:model Http/Models/User
- 模型编码要求数据库是复数s[es,ies]
- return Str::plural('user')返回字符串复数

#### could not driver
window配置：
php.ini：
extension=php_pdo_pgsql.dll  
extension=php_pgsql.dll  
重启apache：d:/apache/apache24/bin:httpd –k restart

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
