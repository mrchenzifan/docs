# 视图

视图组件是采用框架内置模板引擎，暂不支持其他引擎

## 导航

- [配置](#config)
- [简述](#intro)
- [渲染](#view)
- [赋值变量](#assign)
- [流程控制](#contol)
- [运行原生语句](#orginal)
- [加载静态资源](#static)
- [加载其他模板](#template)

## <a id="config">配置</a>

配置主要是在 `config/view.php` 文件配置

主要配置如下：

```php
<?php
/**
 * This file is part of monda-worker.
 *
 * @contact  mondagroup_php@163.com
 *
 */

use framework\view\driver\HeroTemplate;

return [
    //原生模板引擎
    'handler' => HeroTemplate::class,
    // 模板缓存路径
    'cache_path' => runtime_path() . '/view/',

    // 模板的根目录
    'view_path' => BASE_PATH . '/view/',

    /*
     * 模板编译缓存配置
     * 0 : 不启用缓存，每次请求都重新编译(建议开发阶段启用)
     * 1 : 开启部分缓存， 如果模板文件有修改的话则放弃缓存，重新编译(建议测试阶段启用)
     * -1 : 不管模板有没有修改都不重新编译，节省模板修改时间判断，性能较高(建议正式部署阶段开启)
     */
    'cache' => env('VIEW_CACHE', 0),

    //模板文件后缀
    'view_suffix' => 'html', 
];

```

## <a id="intro">简述</a>

视图文件默认使用`.html`后缀， 也可以通过修改配置`view_suffix`的值改变， 调用`view($template, $vars = [])`函数渲染

控制器调用， eg:

```php
//--snip--
    /**
     * 模板引擎
     * @RequestMapping(value="/", method={"get"}, msg="test")
     */
    public function index()
    {
        assign('title', 'heros-worker-demo');
        return view('demo/index');
    }
//--snip--
```

在 `BASE_PATH . '/view/demo'` 目录下创建 `index.html` 文件， eg:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{$title}</title>
</head>
<body>
<h3>welcome, heros-worker framework app demo</h3>
</body>
</html>
```

## <a id="view">渲染</a>

渲染模板使用`view($template, $vars = [])`

- 参数： `$template` 指向模板的位置， 例如模板文件： `BASE_PATH . '/view/demo/index.html'`,  `$template = 'demo/index'`
- 参数： `$vars` 赋值操作， 可以在模板使用此变量

## <a id="assign">赋值变量</a>

使用 `assign($name, $value)` 函数赋值变量， 模板中变量左右需要用`{}` 包裹

- `view($template, $vars = [])` 第二个参数也可以赋值， 在模板中使用方式跟下面一致
- 赋值单一变量， `assign('version', '1.0.0')`, 模板文件中使用, eg:

    ```html
    //--snip--
    <h3>{$version}</h3>
    //--snip--
    ```

- 数组赋值， 暂时值支持一维数组，和二维数据，多层使用`.`拼接

    ```php
    //--snip--
    assign('arr', [
            'name' => 'tom',
            'info' => [
                'age' => 1,
            ]
        ])
    //--snip--
    ```

    模板中使用：

    ```html
    //--snip--
    <h3>name : {$arr.name}, age: {$arr.info.age} {$arr.info.extra.age}</h3>
    //--snip--
    ```

## <a id="contol">流程控制</a>

流程控制主要是对 `if`, `foreach` 进行实现

### 判断语句

- `if` 语句 =》`{if 条件} ... {/if}`, 条件中语法和php语法一致， eg:

    ```html
    //--snip--
    {if isset($name)}
    <h3>hello , {$name}</h3>
    {/if}
    //--snip--
    ```

- `if-else` 语句 =》 `{if 条件} ... {else} ... {/if}`, eg:

    ```html
    //--snip--
    {if ($name === 'tom')}
    <h3>tom, hello</h3>
    {else}
    <h3>unknown user</h3>
    {/if}
    //--snip--
    ```

- `if-elseif` 语句 =》 `{if 条件} ... {elseif} ... {else} ... {/if}`, eg:

    ```html
    //--snip--
    {if ($name === 'tom')}
    <h3>tom, hello</h3>
    {elseif $name === 'cat'}
    <h3>hello, cat</h3>
    {else}
    <h3>unknown user</h3>
    {/if}
    //--snip--
    ```

### 循环

- `for` 语句 `{for 条件} ... {/for}`, eg:

    ```html
    //--snip--
    {for ($i = 0; $i < 3; $i++) }
    <li>item:{$i}</li>
    {/for}
    //--snip--
    ```

- `foreach($data as $key => $value)` 语句 =》 `{loop $data $key $value} ... {/loop}`

    ```php
    //--snip--
        $data = [
            'name' => 'tomcat',
            'age' => 1,
        ];
        assign('data', $data);
    //--snip--
    ```

    模板文件 eg：

   ```html
    //--snip--
    {loop $data $key $value}
    <li>{$key} : {$value}</li>
    {/loop}
    //--snip--
    ```

- `foreach($data as $value)` 语句 =》 `{loop $data $value}` ... `{/loop}`

    ```php
    //--snip--
        $data = [
            [
                'name' => 'tomcat',
                'age' => 1,
            ],
            [
                'name' => 'tomcat2',
                'age' => 12,
            ]
        ];
        assign('data', $data);
    //--snip--
    ```

    模板文件 eg：

   ```html
    //--snip--
    {loop $data2 $user}
    <li>{$user.name} : {$user.age}</li>
    {/loop}
    //--snip--
    ```

## <a id="orginal">运行原生语句</a>

使用 `{run 语句}`， eg： `<h2>{run echo date('Y-m-d H:i:s');}</h2>`

## <a id="static">加载静态资源</a>

使用 `{res:类型 静态资源路径}`， 类新支持 `css`, `less`, `js` 三种类型， 静态资源路径 是指放在项目根目录下`public`文件夹下； 比如  `BASE_PATH . /public/static/js/demo.js` 文件对应的*静态资源路径*是 `static/js/demo.js`

使用eg:

```html
//--snip--
{res:css static/css/demo.css}
{res:js static/js/demo.js}
//--snip
```

## <a id="template">加载其他模板</a>

主要用共用模板，公共模板提取出来，比如头， 导航 等， 使用 `{require 模板路径}`

> 注意： 这里的模板路径规则和 使用`view($template, $vars = [])` *$template* 参数规则一致， 但又有区别，这里使用 `.` 拼接路径， `view()` 参数里面使用 `/` 拼接路径。

eg:

```html
//--snip--
<h1 class="demo"> css</h1>

{require demo.section}
{require demo.view.section2}
//--snip--
```
