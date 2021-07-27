# `Result` 类

框架内置统一格式的返回类, 成功code `'000'`, 失败 `'001'`

## 格式

### 普通格式

key | 类型 | 描述
-|-|-
 code | string | 返回码, 内置：成功code `'000'`, 失败 `'001'`
 success | bool | 状态， 成功 `true`, 失败 `false`
 mssage | string | 信息， 默认 *操作成功\|操作失败*
 data | mixed | 数据， 如果有数据返回，则有这个键值
 extra | mixed | 额外数据， 如果有数据返回，则有这个键值

## 导航

- 返回成功：[`Result::ok()`](#ok)
- 返回失败：[`Result::error()`](#error)
- 设置信息：[`Result::message()`](#error)
- 自定义code: [`Result::code($code)`](#code)
- 返回数据: [`Result::data($data)`](#data)
- 设置额外数据: [`Result::extra($data)`](#extra)
- 分页数据: [`Result::pager($page, $pageSize, $total, $data, $extra = [])`](#pager)

## <a id="ok">成功 `Result::ok`</a>

```php
//--snip--
    /**
     * @return Result
     * @RequestMapping(value="/demo/res/ok")
     */
    public function ok(): Result
    {
        return Result::ok();
    }
//--snip--
```

结果

```json
{
    "code": "000",
    "success": true,
    "message": "操作成功"
}
```

## <a id="error">失败 `Result::error`</a>

```php
//--snip--
    return Result::error()->message('系统出小差了');
//--snip--
```

结果

```json
{
    "code": "001",
    "success": false,
    "message": "系统出小差了"
}
```

## <a id="code">自定义code `Result::code`</a>

```php
//--snip--
    return Result::error()->code('005')->message('系统出小差了');
//--snip--
```

结果

```json
{
    "code": "005",
    "success": false,
    "message": "系统出小差了"
}
```

## <a id="data">返回数据 `Result::data`</a>

```php
//--snip--
    return Result::ok()->data([
            'id' => 1,
            'name' => 'tom'
        ]);
//--snip--
```

结果

```json
{
    "code": "000",
    "success": true,
    "message": "操作成功",
    "data": {
        "id": 1,
        "name": "tom"
    }
}
```

## <a id="extra">设置额外数据 `Result::extra`</a>

```php
//--snip--
    return Result::ok()->data([])->extra(['id' => 1]);
//--snip--
```

结果

```json
{
    "code": "000",
    "success": true,
    "message": "操作成功",
    "data": [],
    "extra": {
        "id": 1
    }
}
```

## <a id="pager">分页数据 `Result::pager`</a>

### 传递参数

- `$page` 页码
- `$pageSize` 页面大小
- `$total` 总数量
- `$data` 单前页面数据
- `$extra = []` 额外数据

```php
//--snip--
    return Result::pager(1, 10, 2, [
            ['id' => 1, 'name' => 'tom'],
            ['id' => 2, 'name' => 'tom2'],
        ]);
//--snip--
```

结果

```json
{
    "code": "000",
    "success": true,
    "message": "操作成功",
    "data": {
        "result": [             //当前页面数据
            {
                "id": 1,
                "name": "tom"
            },
            {
                "id": 2,
                "name": "tom2"
            }
        ],
        "pager": {
            "currentPage": 1,  //单前页码
            "pageSize": 10,    //页面大小
            "total": 2,        //总数量
            "totalPage": 1     //总页数
        }
    }
}
```
