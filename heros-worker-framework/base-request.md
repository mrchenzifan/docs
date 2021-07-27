# Request 请求

请求主要是讲解`Request`类对象，其命名空间`use framework\http\Request;`,包含了http请求的相关信息，主要再是控制器方法中作为接收参数

## 主要方法

- `getConnection()` 获取TcpConnection连接
- `getParams()` 获取所有请求参数
- `getParameter(string $name, $default = null)` 根据key值获取单个参数
- `getRawBody()` 获取原始rowBody数据
- `getHeader()` 获取请求头信息
- `getHeaderByName($name, $default = null)` 根据key值获取头信息
- `getHost()` 获取host
- `getIp()` 获取ip
- `getProt()` 获取prot
- `getMethod()` 获取请求方法GET...
- `getUri()` 获取请求url
- `getPath()` 获取path
- `getFiles()` 获取所有上传文件 具体详见[请求文件处理](heros-worker-framework/base-request-file.md)章节
- `file($name)` 根据name获取上传文件 具体详见[请求文件处理](heros-worker-framework/base-request-file.md)章节
