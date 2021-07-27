# 请求文件处理

对于请求文件的接收和处理

## 获取原始请求文件数据

在控制器方法中通过`$request->getFiles()`获取

- eg:

    ```php
    //--snip--
    /**
     * @param Request $request
     * @RequestMapping(value="/demo/cno/files", method={"post"}, msg="test")
     */
    public function files(Request $request)
    {
        $files = $request->getFiles();
        return $files;
    }
    //--snip--
    ```

    `http://127.0.0.1:8080/demo/cno/files` 放回结果：

    ```json
    {
        "src":{
            "name":"rec33.jpeg",
            "tmp_name":"\/tmp\/workerman.upload.np6i48",
            "size":61591,
            "error":0,
            "type":"image\/jpeg"
        },
        "src2":{
            "name":"table1.jpg",
            "tmp_name":"\/tmp\/workerman.upload.RmbKW9",
            "size":40926,
            "error":0,
            "type":"image\/jpeg"
        }
    }
    ```

## 根据*name*值获取对应的文件

在控制器方法中通过`$request->file('src')`获取, 此方法返回的是一个`UploadFile`上传文件对象(如果不存在则返回`null`)，其命名空间是`framework\http\UploadFile`;

### `UploadFile`对象

此类对上传文件进行了一次封装，使用户更加方便处理上传文件， 此类继承了 `\SplFileInfo` 类， 关于此类可以查阅php官方文档

#### 方法

- `getUploadName()` 获取上传文件名
- `getUploadMineType()` 获取文件类型
- `getUploadExtension()` 获取文件后缀
- `isValid()` 判断文件是否上传成功
- `getSize()` 获取文件大小
- `getPath()` 获取上传文件临时目录

#### 移动/保存文件

使用 `move()` 方法, 成功返回新的`UploadFile`对象， eg:

```php
//--snip--
    $file = $request->file('src');
    $dst = runtime_path() . '/upload/test.' . $file->getUploadExtension();
    $file->move($dst);
//--snip--
```
