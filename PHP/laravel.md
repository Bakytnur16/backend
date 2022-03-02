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
