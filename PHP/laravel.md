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
- 视图路径（三个参数）：url，名称，参数
- 文件在在resource/views/xxx.blade.php 形式创建

- html快速创建： 输入'html:5 +TAB键'

```
Route::view('task','task',['id'=>10]);
```
