# 注解

注解是在类|方法|属性上打上注解， 实现不同的功能， 框架内置了以下几种注解， 方便开发

## 导航

- [@Controller](#Controller)
- [@RequestMapping](#RequestMapping)
- [@Resource](#Resource)
- [@Service](#Service)
- [@Dao](#Dao)
- [@Validator](#Validator)
- [@Config](#Config)
- [@Component](#Component)

## <a name="Controller">`@Controller`</a>

表明是控制器，一般搭配`@RequestMapping`注解以其使用

- 引用的命名空间： `framework\annotations\Controller`
- 作用在类上面
- 属性值:`msg`(非必填)： 备注信息，用于生成 yapi json文件
- eg:

    ```php
    <?php
    namespace app\modules\demo\Controller;

    use framework\annotations\Controller;
    use framework\annotations\RequestMapping;
    use app\modules\demo\service\UserService;
    use framework\annotations\Resource;
    use framework\core\AbstractController;
    use framework\util\Result;

    /**
    * Class UserController
    * @package app\modules\demo\Controller
    * @Controller(msg="")
    */
    class UserController extends AbstractController
    {
        /**
        * @var UserService $service
        * @Resource
        */
        private $service;

        /**
        * @return Result
        * @RequestMapping(value="/demo/user/test", method={"get"}, msg="test")
        */
        public function test(): Result
        {
            return Result::ok();
        }
    }
    ```

## <a id="RequestMapping">`@RequestMapping`</a>

路由注解，定义路由，详见：[路由](heros-worker-framework/base-route.md) 章节

- 引用的命名空间： `framework\annotations\RequestMapping`
- 作用在控制器的方法上
- 属性值:`value`(必填)：路由值， `method`(非必填默认`get`): 允许的请求方法， `msg`(非必填)： 备注信息，用于生成 yapi json文件

## <a id="RequestMapping">`@Resource`</a>

资源注解，表示引用一个注解资源，会自动注入属性的值

- 引用的命名空间： `framework\annotations\Resource`
- 作用在类的属性上
- 先打上 `@var type $name` 再打上 `@Resource`
- eg:

    ```php
    //--snip--
    use app\modules\demo\service\UserService;
    use framework\annotations\Resource;
    //--snip--

    /**
     * $service 会自动被注解赋值 为 UserService 对象
     * @var UserService $service
     * @Resource
     */
    private $service;

    //--snip--
    ```

## <a id="Service">`@Service`</a>

服务注解，表明这个类是一个服务，可以在其他类通过`@Resource`注入此服务, 通常是在*控制器的属性*注入服务

- 引用的命名空间： `framework\annotations\Service`
- 作用在类上
- eg：

    ```php
    <?php
    namespace app\modules\demo\service;

    use framework\annotations\Service;
    use app\modules\demo\dao\UserDao;
    use framework\annotations\Resource;

    /**
    * Class UserService
    * @package app\modules\demo\service
    * @Service
    */
    class UserService
    {
        //--snip
        /**
        * @var UserDao $dao
        * @Resource
        */
        private $dao;
    }
    ```

## <a id="Dao">`@Dao`</a>

dao层类，表明是dao类，通常是在*服务类的属性*注解dao类对象

- 引用的命名空间： `framework\annotations\Dao`
- 作用在类上
- eg：

    ```php
    <?php
    namespace app\modules\demo\dao;

    use framework\annotations\Dao;

    /**
    * Class UserDao
    * @package app\modules\demo\dao
    * @Dao
    */
    class UserDao
    {
        //--snip--
    }
    ```

## <a id="Validator">`@Validator`</a>

验证表单数据，详情使用方法可以详见：[表单验证](heros-worker-framework/base-validate.md) 章节

- 引用的命名空间： `framework\annotations\Validator`
- 作用在**vo类**上
- 属性值： `class`(必填): 验证器类类名， `scence`(必填): 验证场景
- eg：

    ```php
    <?php
    /**
    * This file is part of monda-worker.
    *
    * @contact  mondagroup_php@163.com
    *
    */
    namespace app\modules\demo\vo;

    use framework\annotations\Validator;
    use framework\vo\RequestVoInterface;

    /**
    * 登陆LoginVo
    * @package app\modules\demo\vo
    * @Validator(class="app\modules\demo\validator\IndexValidator", scene="login")
    */
    class LoginVo implements RequestVoInterface
    {
        //--snip--
    }
    ```

##  <a id="Config">`@Config`</a>

注解配置（`config`目录下）属性值

- 引用的命名空间： `framework\annotations\Config`
- 作用在类的属性上
- 属性值： `name`(必填)： 配置的值，多层使用`.`拼接， `default`: 默认值
- eg:

    ```php
    <?php
    namespace app\modules\demo\controller;

    use framework\util\Result;
    use framework\core\AbstractController;
    use framework\annotations\RequestMapping;
    use framework\annotations\Controller;
    use framework\annotations\Config;

    /**
    * Class AnoController
    * @package app\modules\demo\controller
    * @Controller(msg="")
    */
    class AnoController extends AbstractController
    {

        /**
        * @Config(name="app_name")
        */
        private $appName;

        /**
        * @Config(name="xxx", default="tom")
        */
        private $name;

        /**
        * @Config(name="database.default.driver")
        */
        private $dbDriver;

        /**
        * @return Result
        * @RequestMapping(value="/demo/ano/test")
        */
        public function test(): Result
        {
            return Result::ok()->data([
                'appName' => $this->name,
                'name' => $this->name,
                'dbDriver' => $this->dbDriver
            ]);
        }
    }
    ```

- `http://127.0.0.1:8080/demo/ano/test`响应结果`{"code":"000","success":true,"message":"操作成功","data":{"appName":"tom","name":"tom","dbDriver":"mysql"}}`

## <a id="Component">`@Component`</a>

容器组件注解，在类上注解此项，会自动在容器中单例此对象

- 引用的命名空间： `framework\annotations\Component`
- 作用在类的上
- 属性值： `name`(必填)： 对应存储在容器内中的键值, 容器内容祥见[容器](heros-worker-framework/core-container.md) 章节
- eg:

    ```php
    <?php
    namespace app\modules\demo\utils;

    use framework\annotations\Component;

    /**
    * Class Code
    * @package app\modules\demo\utils
    * @Component(name="Code")
    */
    class Code
    {
        /**
        * @return int
        */
        public function getRand(): int
        {
            return rand(0, 100);
        }
    }
    ```

    在其他地方使用:

    ```php
    <?php
    namespace app\modules\demo\controller;

    use app\modules\demo\utils\Code;
    use framework\util\Result;
    use framework\core\AbstractController;
    use framework\annotations\RequestMapping;
    use framework\annotations\Controller;

    /**
    * Class CnoController
    * @package app\modules\demo\controller
    * @Controller(msg="")
    */
    class CnoController extends AbstractController
    {

        /**
        * @return Result
        * @RequestMapping(value="/demo/cno/test", method={"get"}, msg="test")
        */
        public function test(): Result
        {
            /** @var Code $code */
            $code = load("Code");
            return Result::ok()->data([
                'rand' => $code->getRand(),
            ]);
        }
    }
    ```

    `http://127.0.0.1:8080/demo/cno/test` 访问响应结果 `{"code":"000","success":true,"message":"操作成功","data":{"rand":97}}`
