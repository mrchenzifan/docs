# 命名规约

1. **【强制】** 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。

   **<font color='#937c27'>说明：</font>** 正确的英文拼写和语法可以让阅读者易于理解，避免歧义。 注意，即使纯拼音命名方式也要避免采用。

   **<font color='#ec5248'>反例：</font>** `DaZhePromotion` [打折] / `getPingfenByName()` [评分] / `int` 某变量 = 3

   **<font color='#4ead5b'>正例：</font>** `monda / fiidee / dayi `等国际通用的名称，可视同英文。

2. **【强制】** 杜绝完全不规范的缩写，避免望文不知义。

   **<font color='#ec5248'>反例：</font>** `AbstractClass` “缩写”命名成 `AbsClass`；`condition` “缩写”命名成 `condi`，此类随意缩写严重降低了代码的可阅读性。

3. **【强制】** 类文件的命名必须以大写字母开头，其他文件（配置文件，视图，一般的脚本文件等）的命名是全小写。 另外，类文件的名称必须和类的名称保持一致。

   **<font color='#ec5248'>反例：</font>** `user.php`

4. **【强制】** 类的命名必须遵循 `UpperCamelCase` 大写开头的驼峰命名规范。

   **<font color='#4ead5b'>正例：</font>** `**UserDao / XmlService`

   **<font color='#ec5248'>反例：</font>** `macroPolo`

5. **【强制】** 包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。

   **<font color='#4ead5b'>正例：</font>** 应用工具类包名为`com`. `monda` . `open` . `util` 、类名为 MessageUtils`( 此规则参考 spring 的框架结构 )

6. **【强制】** 抽象类命名使用 `Abstract` 或 `Base` 开头；异常类命名使用 `Exception` 结尾； 测试类命名以它要测试的类的名称开始，以 `Test` 结尾。

7. **【推荐】** 如果使用到了设计模式，建议在类名中体现出具体模式。

   **<font color='#937c27'>说明：</font>** 将设计模式体现在名字中，有利于阅读者快速理解架构设计思想。

   **<font color='#4ead5b'>正例：</font>**

   ```php
   public class OrderFactory;
   public class LoginProxy;
   public class ResourceObserver;
   ```

8. **【强制】** 方法名称和属性命名必须符合 `lowerCamelCase` 式的小写开头驼峰命名规范。

   **<font color='#4ead5b'>正例：</font>** `getHttpMessage() / inputUserId / $httpClient / $shopName`

9. **【强制】** 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。

   **<font color='#4ead5b'>正例：</font>** `MAX_STOCK_COUNT`

   **<font color='#ec5248'>反例：</font>** `MAX_COUNT`

10. **【强制】** 接口和实现类的命名有两套规则:

    于 `Service` 和 `DAO` 类，基于 `SOA` 的理念，暴露出来的服务一定是接口，内部的实现类用` Impl `的后缀与接口区别。

    **<font color='#4ead5b'>正例：</font>**`CacheServiceImpl` 实现` CacheService` 接口。

    如果是形容能力的接口名称，取对应的形容词做接口名(通常是`–able` 的形式)。

    **<font color='#4ead5b'>正例：</font>** `AbstractTranslator` 实现 `Translatable`。

11. **【强制】** 各层命名规约，`controller` 层， `service` 层，` dao `层的命名需要以 业务名称 + 层名称 来命名

    **<font color='#4ead5b'>正例：</font>**`UserController / UserService / UserDao`
    
    > `Service/DAO` 层方法命名规约 
    >
    > 1. 获取单个对象的方法用 `get` 做前缀。 
    > 2. 获取多个对象的方法用 `list` 做前缀。 
    > 3. 获取统计值的方法用 `count` 做前缀。
    > 4. 添加的方法用 add 做前缀，`addUser(User $user)`。 
    > 5. 修改的方法用 update 做前缀，`updateUser(User $user)`。 
    > 6. 删除的方法用 delete 做前缀， `deleteUser()`。 
    > 7. 返回布尔值的方法用 is 做前缀， `isValidate()`。 
    > 8. 执行某项具体的业务使用 do 做前缀，如：
    
    ```php
    //注册收集信息的页面是 register() 方法
    public register(HttpRequest $request) {
        //do something
    }
    //那么具体执行注册动作则需要使用 doRegister
    public register(HttpRequest $request) {
        //do something
    }
    
    //审核
    public check(HttpRequest $request) {
        //do something
    }
    
    //审核操作
    public doCheck(HttpRequest $request) {
        //do something
    }
    ```
    
    