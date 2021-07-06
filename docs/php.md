
## PHP 过滤微信昵称表情

```php
    function filterNickname($nickname) {
        $nickname = preg_replace('/[\x{1F600}-\x{1F64F}]/u', '', $nickname);
        $nickname = preg_replace('/[\x{1F300}-\x{1F5FF}]/u', '', $nickname);
        $nickname = preg_replace('/[\x{1F680}-\x{1F6FF}]/u', '', $nickname);
        $nickname = preg_replace('/[\x{2600}-\x{26FF}]/u', '', $nickname);
        $nickname = preg_replace('/[\x{2700}-\x{27BF}]/u', '', $nickname);
        $nickname = str_replace(array('"', '\''), '', $nickname);
        return addslashes(trim($nickname));
    }
```

## Laravel
> 创建项目 安装 laravel

```php
composer create-project --prefer-dist laravel/laravel laravel "5.5.*"
```

> 安装代码提示

```php
composer require barryvdh/laravel-ide-helper
// 在 「config/app.php」的 「providers」数组中加入
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class
    
//为 Facades 生成注释
php artisan ide-helper:generate
// 为数据模型生成注释
php artisan ide-helper:models
//	生成 PhpStorm Meta file
php artisan ide-helper:meta
```

> 查看路由

```php
php artisan route:list
```

> 创建控制器

```php
php artisan make:controller IndexController
```

>创建模型文件

```php
php artisan make:model Article
    
//	Laravel会在app目录下生成一个Article.php的模型文件。但是我们为了方便，一般会将模型文件放在Model目录下，所以需要在生成文件的时候指定命名空间
 
php artisan make:model Models/Article

//	Laravel会自动生成Models目录和Article.php文件，如果你想在生成模型文件的同时生成迁移文件，可以在后面加上-m
 
php artisan make:model Models/Article -m
 
//	参数配置
//	模型文件采用单数形式命名，而数据表采用复数形式命名。所以一个Article模型默认对应Articles 数据表，如果我们在开发中需要指定表的话。
 
//关联到模型的数据表
protected $table = 'article2';
 
//指定主键
protected $primaryKey = 'article_id';
 
//表明模型是否应该被打上时间戳
public $timestamps = true;
 
//模型日期列的存储格式
protected $dateFormat = 'U';
 
/**
     * 可以被批量赋值的属性.
     * @var array
*/
protected $fillable = ['name'];
 
//隐藏字段
protected $hidden = ['password'];
/**
	* 不能被批量赋值的属性
*/
protected $guarded = ['price'];

```

> 创建数据库表结构

```php
//	创建数据库表
php artisan make:migration create_users_table
//	数据库运行迁移
php artisan migrate
//	创建一个填充文件，并完善填充文件
php artisan make:seeder WechatTableSeeder
//	执行默认数据填充
php artisan db:seed
//	执行单个填充文件
php artisan db:seed --class=WechatTableSeeder
//	清空默认的数据 ，重新写入
php artisan migrate:refresh --seed
```

> 数据库操作

```php
//	插入数据
DB::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);

// 查询数据
$users = DB::table('users')->get();
$user = DB::table('users')->where('name', 'John')->first();
$email = DB::table('users')->where('name', 'John')->value('email');
//更新
DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);
```

> 创建数据库低版本  utf8mb4_unicode_ci

```php
// app/Providers/AppServiceProvider.php

use Illuminate\Support\Facades\Schema;

/**
 * 引导任何应用程序服务
 *
 * @return void
 */
public function boot()
{
    Schema::defaultStringLength(191);
}
```

> 数据库测试 生成模拟数据

```php

$factory->define(App\User::class, function (Faker\Generator $faker) {
    static $password;

    return [
        'name' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => $password ?: $password = bcrypt('secret'),
        'remember_token' => str_random(10),
    ];
});



/**
 * 管理员表 填充数据
 * use App\Models\Admin;
 * use Illuminate\Support\Str;
 * use Faker\Generator as Faker;
 */
$factory->define(Admin::class, function (Faker $faker) {
    static $password;
    return [
        'username' => 'admin',
        'email' => $faker->unique()->safeEmail,
        'password' => bcrypt('admin'),
        'remember_token' => Str::random(10),
    ];
});

// 执行填充 一条数据
factory(App\Models\Admin::class,1)->create(); 


php artisan tinker;

// 创建 3 个 App\User 实例
$users = factory(App\User::class, 3)->create();

```

> 用户登录验证

```php
use Auth;

public function login(Request $request)
{
    $data = [
        'username'=>$request->username,
        'password'=>$request->password,
    ];
    $res = Auth::guard('admin')->attempt($data);

    if($res){
        return [
            'code'=>0,
            'msg'=>'登录成功',
            'url'=>route('admin.index')
        ];
    }
    return [
        'code'=>1,
        'msg'=>'帐号或密码错误'
    ];
}
```

>  中间件

```php
//	 创建中间件
php artisan make:middleware CheckAge
//	路由分配多个中间件：

Route::get('/',function(){
　　//
})->middleware('first','second');

Route::group(['middleware'=>['web']],function(){
　　//
})

//	app/Http/Kernel.php 配置中间件
   
```

> 时间戳  修改laravel的时区设置 

```php

//  默认使用时间戳戳功能
public $timestamps = true;
//  模型日期列的存储格式
protected $dateFormat = 'U';

//目录修改	app\config\app.php
'timezone' => 'PRC',

$res = Banner::find(58);
echo $res->created_at->format('Y-m-d H:i:s');
```

>  Laravel 内置的 Eloquent ORM  

```php
/**
* 查询所有数据
*/
$res = Banner::all();
/**
* 根据ID 查询数据
*/
$res = Banner::find(1);
$res = Banner::findOrfail(10);
$res = Banner::where("id", "<", "10")->first();
$res = Banner::all()->min('id');
$res = Banner::count();
/**
* 新增数据
*/
$res = New Banner();
$res->title = rand(1, 9999999);
$res->state = '1';
$bool = $res->save();
dd($bool);

// 指定可以批量设置字段
protected $fillable = ['title','state'];
Banner::create(['title' => '123123', 'state' => '1']);


// 通过属性获取航班, 如果不存在则创建...
$flight = App\Flight::firstOrCreate(['name' => 'Flight 10']);

// 通过name获取航班，如果不存在则通过name和delayed属性创建...
$flight = App\Flight::firstOrCreate(
    ['name' => 'Flight 10'], ['delayed' => 1]
);

// 通过属性获取航班, 如果不存在初始化一个新的实例...
$flight = App\Flight::firstOrNew(['name' => 'Flight 10']);

// 通过name获取，如果不存在则通过name和delayed属性创建新实例...
$flight = App\Flight::firstOrNew(
    ['name' => 'Flight 10'], ['delayed' => 1]
);


// 更新数据
// 如果没有匹配的模型则创建之
$flight = App\Flight::updateOrCreate(
    ['departure' => 'Oakland', 'destination' => 'San Diego'],
    ['price' => 99]
);


// 删除数据
$flight = App\Flight::find(1);
$flight->delete();

App\Flight::destroy(1);
App\Flight::destroy([1, 2, 3]);
App\Flight::destroy(1, 2, 3);

$deletedRows = App\Flight::where('active', 0)->delete();
```

> 模板继承Blade

```php
@section
@yield
@extends
@prent
```

> 全局 Session 辅助函数

```php
Route::get('home', function () {
    // 从session中获取数据...
    $value = session('key');
    // 指定默认值...
    $value = session('key', 'default');
    // 存储数据到session...
    session(['key' => 'value']);
});
```

> 