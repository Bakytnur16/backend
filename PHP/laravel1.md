增删改查+循环+判断  
php artisan serve 修改了配置文件需要重启  
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
- 

