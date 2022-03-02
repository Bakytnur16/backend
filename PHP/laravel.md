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
Route::any()接受任何响应；
Route::match()接受特定的提交方式：

Route::match(['get','post'],'index',function() //必须有三个参数
{
  return 'hello world';
});

```
###控制器：接受http请求
- MVC里的C
- 创建控制器： php artisan make:controller TaskController
```
Route::get('/task',[TaskController::class, 'index']);//参数二：控制器@方法名
Route::get('/task/read/{id}',[TaskController::class, 'read']); //参数设置
Route::get('task','App\Http\Controllers\TaskController@index');


```



## 视图路由:views
```
- MVC中的view
- 文件在在resource/views/xxx.blade.php 形式创建

- html快速创建： 输入'html:5 +TAB键'
