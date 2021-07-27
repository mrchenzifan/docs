# vo类

vo类一般用于请求的数据映射，使用对象要比使用数组传递、使用方便。*vo*类中的属性应该和请求的数字字段名一一对应，类中只用属性的`getter` 和 `setter` 方法。

> 一般是搭配注解[`@Validator`](heros-worker-framework/base-annotations.md?id=Validator)让框架字使用验证器自动验证请求数据， 详见[表单验证](heros-worker-framework/base-validate.md)

## 数组和vo类的转换

使用 `framework\util\ModelTransformUtils` 类中的方法进行转换

- `ModelTransformUtils::map2Model(string $class, array $map = []): object`, 数组中数据复制到类中属性
- `ModelTransformUtils::model2Map($model): array`, 对象的属性转化为数组返回
