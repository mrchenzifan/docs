# 常量

1. **【强制】** 不允许出现任何魔法值 ( 即未经定义的常量 ) 直接出现在代码中。

   **<font color='#937c27'>说明：</font>** 以字面常量取代魔法值，创建一个常量，根据其意义为它命名，并将上述的字面值替换为这个常量。

   **<font color='#ec5248'>反例：</font>**
   
   ```php
   function potentialEnergy($mass, $height){
   
       return mass*9.81*height;
   }
   ```
   
   **<font color='#4ead5b'>正例：</font>**
   
   ```php
   function potentialEnergy($mass, $height){
       return mass*GRAVITATIONAL_CONSTANT*height;
   }
   
   static final GRAVITATIONAL_CONSTANT=9.81;
   ```
   
   

2. **【推荐】** 不要使用一个常量类维护所有常量，应该按常量功能进行归类，分开维护。如:缓存相关的常量放在类: `CacheConsts` 下; 系统配置相关的常量放在类: `ConfigConsts` 下。

   **<font color='#937c27'>说明：</font>** 大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。



# 数组

1. **【强制】** 大的数组在使用完之后如果业务还没有结束，需要手动销毁变量释放内存，使用 `unset($array)`;

   **<font color='#937c27'>说明：</font>** 数组和对象在`php`特别占内存的，这个由于`php`的底层的`zend`引擎引起的， 一般来说，`PHP`数组的内存利用率只有 `1/10`， 也就是说，一个在`C`语言里面`100M 内`存的数组，在`PHP`里面就要`1G`。 特别是在`PHP`作为后台服务器的系统中，经常会出现内存耗费太大的问题。

2. **【强制】** 多维数组尽量不要循环嵌套赋值