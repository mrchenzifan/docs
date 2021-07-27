# aop

切面， 可以在某个类方法前后切入某些逻辑， 又不用原类方法上做出改变。

## 生成

> 快捷创建： `php artisan make:aspect -path=app/aspect -class=UserAspect`， 生成代码eg:

```php
<?php
namespace app\aspect;

use framework\aop\AbstractAspect;
use framework\aop\interfaces\ProceedingJoinPointInterface;

/**
 * Class UserAspect
 * @package app\aspect
 */
class UserAspect extends AbstractAspect
{
    public $classes = [
        //classes
    ];

    /**
     * @param ProceedingJoinPointInterface $entryClass
     * @return mixed
     */
    public function process(ProceedingJoinPointInterface $entryClass)
    {
        //doSomething
        $res = $entryClass->process();
        //doSomething
        return $res;
    }
}
```

- `$classes` 是切入的类方法， 目前支持一种写法， *类名::方法名*
- `process()` 是对切入方法的逻辑代码

## 配置

像自定义命令，要使其生效，需要现在 `config/aop.php` 文件中配置， eg:

```php
<?php
return [
    'aspect' => [
        \app\aspect\UserAspect::class,
    ],
];
```

## 实践

修改 `UserAspect` 文件， eg:

```php
//--snip--
    public $classes = [
        UserService::class . '::info'
    ];

    /**
     * @param ProceedingJoinPointInterface $entryClass
     * @return mixed
     */
    public function process(ProceedingJoinPointInterface $entryClass)
    {
        print_line('UserAspect before');
        $res = $entryClass->process();
        print_line('UserAspect after');
        return $res;
    }
//--snip--
```

`UserService.php` code eg:

```php
//--snip--
    public function info()
    {
        echo 'UserService info' . PHP_EOL;
    }
//--snip--
```

- 测试,在 `TestCommand.php` 修改代码 eg：

```php
//--snip--
    public function run(Input $input = null): void
    {
        /** @var UserService $userService */
        $userService = load(UserService::class);
        $userService->info();
    }
//--snip--
```

在命令行窗口键入（项目根目录）：`php artisan test`， 输出结果：

```txt
UserAspect before 
UserService info
UserAspect after 
```

> 注意： 对于切入的类， 必须使用 `load($class)` 函数加载类， 通过此函数加载类， 是会返回类的代理类， 也就会运行切面类的切入逻辑。如果直接 *new* 出来的类是原始类，不会被代理

## 切入顺序

如果有多个切面类对同一个类方法进行切入， 会按照配置文件中顺序执行， eg:

```php
<?php
return [
    'aspect' => [
        \app\aspect\TestAspect::class,
        \app\aspect\UserAspect::class,
    ],
];
```

那么运行刚才的命令测试，输出结果为：

```txt
UserAspect before 
TestAspect before 
UserService info
TestAspect after 
UserAspect after 
```
