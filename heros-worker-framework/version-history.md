# 版本历史记录

## [v1.0.10](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.10) - 2021/08/09
- [c280979](https://github.com/mondagPHP/heros-worker/commit/c280979e057eb8535bf4dc679fefc6883ee1d19b)数据库的心跳默认是关闭，只有database.php 设置了 is_beat = `true` 才会打开。这个开发者必须要谨慎，如果打开请考虑数据库连接数，应用的连接数 = 进程数 * 配置文件数据库连接数 + 定时任务数 * 配置文件数据库连接数

## [v1.0.9](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.8) - 2021/07/26
- [#2007fa](https://github.com/mondagPHP/heros-worker/commit/2007fa900115f996fb4907a68c36b290ccbd12da)增加心跳可选配置，可以指定是否使用数据库心跳。 

## [v1.0.8](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.8) - 2021/07/07
- [#6e693b](https://github.com/mondagPHP/heros-worker/commit/6e693bcf3b8b4f2dd1a7b2b20be0f4453a0d3c32)在max_request_count的数据量，优雅的重启worker，防止内存泄露。 

## [v1.0.7](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.7) - 2021/06/18
- [#8a175b](https://github.com/mondagPHP/heros-worker/commit/8a175b3f0288f674d43f7d0fbd91215933c449b1)增加sse推送组件。
- [#0d225a](https://github.com/mondagPHP/heros-worker/commit/0d225a05bc488125880ef52823343bc5697b57ed)
修改控制器vo在容器内闭包别名存储，防止使用 load(vo::class) 冲突 

## [v1.0.6](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.6) - 2021/06/11
- [#2e899f](https://github.com/mondagPHP/heros-worker/commit/2e899fea7b35f226c597e6cce9d545e70b04a655)修复 make:entity 默认类名问题引起的bug 

## [v1.0.5](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.5) - 2021/06/10
- [#50f668](https://github.com/mondagPHP/heros-worker/commit/50f6680f251ce7f795e7459d1b2e8e76c35e8ee3)修复Response默认头属性：Content-Type, 使用workerman的响应默认头 

## [v1.0.4](https://github.com/mondagPHP/heros-worker/releases/tag/v1.0.4) - 2021/06/09
- [#cdcf3a](https://github.com/mondagPHP/heros-worker/commit/cdcf3a87b64fb24bcee0bd9145a88f2051fa9d51)帮助函数添加`packResult`函数 
- [#901d2](https://github.com/mondagPHP/heros-worker/commit/901d260331ecf8992ae05d2b6009d7fd1284a379)优化`Response::redirect`方法, 默认status 302 
- [#49540](https://github.com/mondagPHP/heros-worker/commit/4954033c081c13eacd128eb57d46c18e4e53fad4)优化`make:entity` 
  - 选项class传值问题， 不传默认table作为类名
  - 新建的entity增加 $connection 属性