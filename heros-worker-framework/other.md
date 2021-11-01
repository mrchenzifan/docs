## 单元测试

### 编写单元测试

单元测试文件名必须以`Test.php`结尾，例如`tests/Unit/HelperTest.php`.

```php
<?php
/**
 * This file is part of monda-worker.
 *
 * @contact  mondagroup_php@163.com
 *
 */
namespace Test\Unit;

use PHPUnit\Framework\TestCase;

class HelperTest extends TestCase
{
    public function testAll()
    {
        $this->assertTrue(all([]));
        $this->assertTrue(all([1, 2, 3, 4, 5, 6]));
        $this->assertTrue(all([-1, -2, -3, -4, -5, -6]));
        $this->assertFalse(all([0, 1, -1 , '1']));
        $this->assertFalse(all(['', 1, -1 , '1']));
        $this->assertFalse(all([null, 1, -1 , '1']));
        $this->assertFalse(all([false, 1, -1 , '1']));
    }

    public function testAny()
    {
        $this->assertFalse(any([]));
        $this->assertFalse(any([0, '', false, null]));
        $this->assertTrue(any([0, '', false, 1]));
        $this->assertTrue(any([0, '1', false, null]));
        $this->assertTrue(any([0, '', true, null]));
        $this->assertTrue(any([1, '', false, null]));
    }
}

```

单元测试框架文档参考: [phpunit 7文档](https://phpunit.readthedocs.io/zh_CN/latest/)

### 运行单元测试

```shell
composer test
```

## 代码语法检测

```shell
composer analyse
```

### 美化代码

```shell
composer cs-fix
```