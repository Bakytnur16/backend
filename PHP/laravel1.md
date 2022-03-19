注意： LTS(长期支持）

### 路由规则：route提供给控制器
- 请求方式（‘请求路由’，匿名函数或控制器响应)
- 必选参数{参数}，可选参数{参数?}
- match(匹配)指定多个路由,any(任何）：
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
- 接受用书输入的类：Illuminate\Support\Dacades\Input
- Facades：门面的思想，门面介于一个类的实例化和没有实例化中间的状态。
```
可以获取get和post
Input::all() 
Input::get()
Input::only([]) 获取指定
Input::expect([]) 取反
Input::has('name') 判断某个输入是否存在
```
