# 容器

容器`Container` 通过*键值*设置对象/其他值， 通过*键值*获取设置的值，框架内容器设置的值都是单例模式, 命名空间是`framework\bootstrap\Container`
> tips: 框架容器目前主要是存储类对象， 类名作为键值

## 设置值

- `container()->set($key, $value)`

## 通过键值获取值

- 方法1： `container()->get($key)`
- 方法2： 通过帮助函数 `load($key)`, 比如加载一个类：`load(UserService::class)`

> 在`load()`主要通过类名加载一个类

## 判断容器内键值是否设置

- `Container::has($key)`
