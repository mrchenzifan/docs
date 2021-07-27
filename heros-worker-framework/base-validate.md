# 表单验证

通过继承 `framework\validate\Validate` 验证器， 定义 `$rule` | `$message` | `$scene` 属性来对数据进行验证， 对于验证不通过会抛出 `framework\exception\ValidateException` 异常

## 验证器

验证器代码 eg:

```php
<?php

namespace app\modules\demo\validator;

use framework\validate\Validate;

/**
 * IndexAction 验证器
 * Class IndexLoginValidator
 * @package app\modules\demo\validator
 */
class IndexValidator extends Validate
{
    //规则
    protected $rule = [
        'account' => 'require',
        'password' => 'require',
    ];

    //信息
    protected $message = [
        'account.require' => '参数account缺少',
        'password.require' => '参数password缺少',
    ];

    //建议方法名称对应
    protected $scene = [
        'login' => ['account', 'password'],
    ];
}
```

## 使用

- `$rule` 是一维数组， *键*是需要验证的字段名， *值* 是验证规则， 多个用 `|` 符号隔开
- `$message` 是验证不通过抛出异常的消息， *键* 是由*字段*和*验证规则*使用`.`拼接， 值是消息
- `$scene` 是验证场景， *键* 是控制器的方法， *值* 是需要验证字段数组
- 在[vo类](heros-worker-framework/utils-vo.md)中打上注解 [`@Validator`](heros-worker-framework/base-annotations.md?id=Validator)， 可以点击链接详见这些章节\
并在控制器方法参数中明确定义 *vo* 对象参数,框架就会自动验证前端提交的表单数据, vo中注解 eg：

    ```php
    <?php
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
        private $account;
        private $password;

        /**
        * @return mixed
        */
        public function getAccount()
        {
            return $this->account;
        }

        /**
        * @param mixed $account
        */
        public function setAccount($account): void
        {
            $this->account = $account;
        }

        /**
        * @return mixed
        */
        public function getPassword()
        {
            return $this->password;
        }

        /**
        * @param mixed $password
        */
        public function setPassword($password): void
        {
            $this->password = $password;
        }
    }
    ```

    在控制器方法中作为参数接收, 框架会自动验证请求的数据，并把数据赋值到vo中，eg:

    ```php
    //--snip--
    /**
     * 登陆
     * @param LoginVo $vo
     * @param Session $session
     * @return Result
     * @RequestMapping(value="/demo/index/login", method={"post", "get"}, msg="login")
     */
    public function login(LoginVo $vo, Session $session): Result
    {
        $data = [
            'account' => $vo->getAccount(),
            'password' => $vo->getPassword(),
        ];
        $session->set('loginUser', StringUtils::jsonEncode($data));
        return Result::ok()->data($data);
    }
    //--snip--
    ```

- 不作为控制器方法参数， 自己主动验证， 框架会自动吧请求数据作为验证数据

    ```php
    //--snip--
    public function login(Session $session): Result
    {
        $validate = new IndexValidator();
        $validate->scene('login')->valid();
        //--snip--
    }
    
    //--snip--
    ```

### 验证器规则

在 `$rule` 属性定义， 键值对方式， *验证字段名称 => 验证规则[|验证规则 ... ]* ,多个使用规则 `|` 隔开 eg：
`

规则|默认消息
-|-
require | :attribute不能为空
number | :attribute必须是数字
integer | :attribute必须是整数
float' | attribute必须是浮点数
boolean | :attribute必须是布尔值
email | :attribute格式不符
array | :attribute必须是数组
accepted | :attribute必须是yes、on或者1
date | :attribute格式不符合
file | :attribute不是有效的上传文件
image | :attribute不是有效的图像文件
alpha | :attribute只能是字母
alphaNum | :attribute只能是字母和数字
alphaDash | :attribute只能是字母、数字和下划线_及破折号-
activeUrl | :attribute不是有效的域名或者IP
chs | :attribute只能是汉字
chsAlpha | :attribute只能是汉字、字母
chsAlphaNum | :attribute只能是汉字、字母和数字
chsDash | :attribute只能是汉字、字母、数字和下划线_及破折号-
url | :attribute不是有效的URL地址
ip | :attribute不是有效的IP地址
dateFormat | :attribute必须使用日期格式 :rule
in | :attribute必须在 :rule 范围内
notIn | :attribute不能在 :rule 范围内
between | :attribute只能在 :1 - :2 之间
notBetween | :attribute不能在 :1 - :2 之间
length | :attribute长度不符合要求 :rule
max | :attribute长度不能超过 :rule
min | :attribute长度不能小于 :rule
after | :attribute日期不能小于 :rule
before | :attribute日期不能超过 :rule
expire | 不在有效期内 :rule
allowIp | 不允许的IP访问
denyIp | 禁止的IP访问
confirm | :attribute和确认字段:2不一致
different | :attribute和比较字段:2不能相同
egt | :attribute必须大于等于 :rule
gt | :attribute必须大于 :rule
elt | :attribute必须小于等于 :rule
lt | :attribute必须小于 :rule
eq | :attribute必须等于 :rule
regex | :attribute不符合指定规则
method | 无效的请求类型
fileSize | 上传文件大小不符
fileExt | 上传文件后缀不符
fileMime | 上传文件类型不符
