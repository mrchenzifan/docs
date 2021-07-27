# 数据库

数据库主要是采用`laravel-orm`, 模型具体操作可以参考[laravel数据库](https://learnku.com/docs/laravel/6.x/eloquent/5176#eloquent-model-conventions)文档

> artsan命令`make:entity`快速生成实体文件 ， eg: `php artisan make:entity -path=app/entity -table=demo_user` 即会在`app/entity`目录下生成`DemoUser`文件， 使用方法详见 [make:entity](heros-worker-framework/command.md?id=entity) 章节

生成代码如下：

```php
<?php

namespace app\entity;

use Carbon\Carbon;
use framework\database\HeroModel;
/**
 * Class DemoUser
 * @property int $id 
 * @property string $name 用户名称
 * @property string $account 账户
 * @property string $password 密码
 * @property Carbon $created_at 
 * @property Carbon $updated_at 
 */
class DemoUser extends HeroModel
{
    protected $table = 'demo_user';
    /** @var array $fillable */
    protected $fillable = [
        //Format each item with a new line
        'id',
        'name',
        'account',
        'password',
    ];
}
```

## 配置

主要是在 `config/database.php` 中配置

## 实体模型

需要继承框架`HeroModel`类（`use framework\database\HeroModel`), 接下来就可以对模型进行操作, eg:

```php
    //获取所有
    DemoUser::query()->getAll();
```

> 模型具体操作可以参考[laravel数据库](https://learnku.com/docs/laravel/6.x/eloquent/5176#eloquent-model-conventions)文档

## 对比 laravel 中 `DB`

在 *laravel* 中， 可以直接使用 `DB::insert()`, 在 *heros-worker* 这里需要稍微变动一下， 变成 `HeroDB::connection('default')->insert()`, 需要先指定连接再调用方法
