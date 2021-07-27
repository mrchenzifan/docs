# 其他规约

1. **【强制】** 每个源文件不应该超过`4000`行，每个方法体不应该超过`150`行代码(不包含 注释 )。

2. **【强制】** 一行只声明一个变量。一个变量有且只有一个功能，不能把一个变量用于多种用途。 变量声明应注意其生命周期，最好定义在变量作用范围代码段的开始部分。最好不要下一行要用，上一行才声明。

   **<font color='#ec5248'>反例：</font>**

	```php
	int $level, $size;
	public function myMethod() {
       $myFlag = false;
       $myFlag = isVisible($myView)//作为可见性判断标志
       //使用 myFlag 做其它操作
       ......
       //作为可用性判断标志(这里应该重新定义一个变量表示)
       $myFlag = isEnable($myComponent)
	}
	```

3. **【强制】** `git`提交的时候的时候需要写明改动备注，项目发布版本的时候需要写比较详细的升级记录，比如解决了什么`bug`，新增了什么功能等。提交备注必须严格按照：`operation + comment`的格式，`operation`代表操作，`comment`表示具体的改动，如：

   ```bash
   add : 新增用户注册功能
   update : 更新用户注册流程
   fixed : 修复了 xxx bug
   remove : 移除了用户日志功能模块
   ```

4. **【推荐】** 控制器是充分体现系统业务功能的地方。好的控制器不宜写入过于复杂的逻辑，应条理清晰，注释到位，要充分考虑封装、重用等问题。

5. **【强制】** 后台发出通过重定向时，`url`参数含有字或母数字外的其它字符应该进行编码处理，避免接收参数值发生错误(中文乱码，特殊符号无法正确传递等)。另外重定向后也应该加入`die()`或者`exit()`;

   **<font color='#4ead5b'>正例：</font>**

   ```php
   header("location:/user/login?cc=".urlencode('你好！'));
   die();
   ```

6. **【强制】** `PHP keywords`必须使用小写。`PHP`常量`true`，`false`和`null`必须使用小写。



### 类和函数规约

1. **【强制】** 一个文件中只写一个类，所有`PHP`文件必须以一个空白行作为结束。纯`PHP`代码文件必须省略最后的`?>`结束标签。

2. **【强制】** 一个类的`extends`和`implements`关键词必须和类名在同一行。`implements`一个列表可以被拆分为多个有一次缩进的后续行。如果这么做，列表 的第一项必须要放在下一行，并且每行必须只有一个接口。

   **<font color='#4ead5b'>正例：</font>**

   ```php
   class ClassName extends ParentClass implements ArrayAccess, Countable {
       // constants, properties, methods 
   }
   
   //或者这样也是可以的
   class ClassName extends ParentClass implements ArrayAccess,
       Countable,
       Serializable {
       // constants, properties, methods
   }
   ```

3. **【强制】** 关于类的属性

   - 所有的属性必须声明可见性。

   - `var`关键词不可用来声明属性。

   - 一个语句不可声明多个属性。

   - 属性名称可以使用单个下划线作为前缀来表明保护或私有的可见性。

   **<font color='#4ead5b'>正例：</font>**

   ```php
   class ClassName {
       public $foo = null;
       private $_bar = 1; 
   }
   ```

4. **【强制】** 关于函数和方法

   - 所有的方法必须声明可见性。
   - 方法名不应只使用单个下划线来表明是保护或私有的可见性。
   - 方法名在声明之后不可跟随一个空格。左花括号放在下面自成一行或者紧跟小括号，并且右花括号必须放在方法主体的下面自成一行。 左括号后面不可有空格，右括号前面不可有空格。
   - 一个方法定义看来应该像下面这样。 注意括号，逗号，空格和花括号。
   - 在参数列表中，逗号之前不可有空格，逗号之后必须要有一个空格。
   - 方法中有默认值的参数必须放在参数列表的最后面。
   - 参数列表可以被分为多个有一次缩进的多个后续行。如果这么做，列表的第一项必 须放在下一行，并且每行必须只放一个参数。
   - 当参数列表被分为多行，右括号和左花括号必须夹带一个空格放在一起自成一行。
   - 如果存在`abstract`和`final`声明必须放在可见性声明前面。
   - 如果存在`static`声明必须跟着可见性声明。

   **<font color='#4ead5b'>正例：</font>**

   ```php
   class ClassName {
       public function fooBarBaz($arg1, &$arg2, $arg3 = []) {
           // method body
       } 
   }
   
   //或者
   class ClassName {
       public function fooBarBaz($arg1, &$arg2, $arg3 = []) {
           //如果是这样写，第一行必须空行
           // method body 
       }
   }
   
   class ClassName {
       public function aVeryLongMethodName( 
           ClassTypeHint $arg1,
           &$arg2,
           array $arg3 = []
       ){
           //method body
       } 
   }
   ```

   