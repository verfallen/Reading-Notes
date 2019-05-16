# 验证方式

- 客户端

  - JavaScript 自定义验证
  - HTML5 内置校验

- 服务器端校验

## 使用内置表单数据验证

- `required`属性
- `pattern`属性，设置正则表达式
- `maxlength`和`minlength`，限制长度：

## 自定义错误信息`setCustomValidity()`

```javascript
email.addEventListener("input", function(event) {
  if (email.validity.typeMismatch) {
    email.setCustomValidity("I expect an e-mail, darling!");
  } else {
    email.setCustomValidity("");
  }
});
```

## 自定义验证不同过样式

可以通过 CSS 伪类 `:valid` 进行特殊的样式化；

## HTML5 校验 API

属性：

- `validateMessage`校验不通过时文本信息
- `validity`描述元素验证状态的对象

  - 数字相关（3 种）
    - `rangeOverflow` 高于最大值
    - `rangeUnderflow`低于最小值
    - `stepMismatch` 步长不符合
  - 与通用属性相关（4 种）
    - `valueMissing` 设置`required`但置为空
    - `patternMismatch`是否不匹配自定义正则
    - `tooLong`太长
    - `typeMismatch`值出现语法错误
  - 结果
    - `valid`值验证通过
  - 其他
    - `customError`是否设置自定义错误
    - `willValidate` 表单在提交时被校验

方法：

- `checkValidity()`验证是否有效
- `setCustomValidity()`添加自定义错误消息
