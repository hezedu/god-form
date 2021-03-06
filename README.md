# vue-form-data
Vue表单组件，能够在数据层验证，没有样式。

## Install
`npm install @hezedu/vue-form`
## 环境
**peerDependencies**: `jade`, `vue-loader`.

**Webpack Config** ：本项目是未经编译的原文件。所以配置rule时不要将本顶目exclude在外。

## API

### 引用
```js
import validators { VForm, VInput, VSubmit } from '@hezedu/vue-form';
```

### validators
Object, 所有验证器(系统自带一个)。可自己扩展。

例:
```js
validators.positiveInteger = function(v){
  if(/\d+/.test(v)){ //v 为用户输入的值。
    return '请输入正整数' //错误
  }else{
    return; //正确
  }
}
```

### components
### VForm props
- `data`: Object 表单数据，该对象会改变属性，请使用数据的深拷贝。
- `validate`: Object 需验证的配置。key改须跟data的key对应。

例:
``vue
<VForm data = '{name: ""}', validate= '{name: "reqired"}' />
```
### VInput props
- `name`: String 必须。
- `type`: String 默认`text`
- `value`: String 非必须。通常只有当`type`为**checkbox**或**radio**时才需要填。其它填了也没用。它会自动绑定到VForm的`data[name]`。

例:
``vue
<VInput name='name' />
```

### VSubmit props
`success(data)` Function 必须。data<Object>为用户输入后的数据。用户输入完成且全部验证都通过，点击触发,获取修改后的`data`

### 注意
**VInput**和**VSubmit**的父组件必须是**VForm**

### Css className
- `.hezedu-submit-disabled` 验证未完全能过时，提交按钮.
- `.hezedu-input-validate-init` 验证初始化(一开始)，input.
- `.hezedu-input-validate-success` 验证成功，input.
- `.hezedu-input-validate-error` 验证失败，input.


## 完整示例：
```vue
<style>
label>span{color: red}
.inline{display: inline-block;}
.hezedu-submit-disabled{color: #999}
.hezedu-input-validate-success>:nth-child(1){
  border-color: green;
}
.hezedu-input-validate-error>:nth-child(1){
  border-color: red;
}
.hezedu-input-validate-init>:nth-child(1){
  border-color: blue;
}
.hezedu-input-validate-success,
.hezedu-input-validate-error,
.hezedu-input-validate-init>:nth-child(2){
  color:red;
}
</style>
<template lang="jade">
VForm(:data='data', :validate='validate')
  table
    tr
      td
        label
          span *
          | 姓名(text):
      td
        VInput(name='name')
    tr
      td
        label
          span *
          | 年龄(number):
      td
        VInput(name='age' type='number')
    tr
      td
        label 性别(radio):
      td(style='width: 200px;display:flex')

        label
          VInput(name='sex' class='inline' value='1' type='radio')
          |男
        label
          VInput(name='sex' class='inline' value='2' type='radio')
          |女
    tr
      td
        label 描述(textarea):
      td
        VInput(name='textarea' class='haha' type='textarea')
    tr
      td
        label 兴趣(checkbox):
      td
        label
          VInput(name='interest' class='inline' value='1' type='checkbox')
          |跑步
        label
          VInput(name='interest' class='inline' value='2' type='checkbox')
          |骑车
        label
          VInput(name='interest' class='inline' value='3' type='checkbox')
          |写代码
    tr
      td
        label
          span *
          |职业(select):
      td
        VInput(name='job' class='haha' type='select')
          option(value='') 请选择
          option(value='1') UI
          option(value='2') 前端
          option(value='3') 后端
  hr
  VSubmit(:success = 'success')
    |submit
    template(scope="props" slot='process')
      span {{props.successTotal}} / {{props.validateTotal}}
  pre {{result}}
</template>

<script>
import {VForm, VInput, VSubmit, ruleExtend} from '@hezedu/vue-form';

export default {
  components: {
    VForm,
    VInput,
    VSubmit
  },
  data(){
    return {
      data: {
        name: '',
        age: '2',
        textarea: '',
        job: '2',
        sex: '1',
        interest: ['1', '2']
      },
      validate: {
        name: 'notEmpty',
        age: 'notEmpty',
        job: 'notEmpty'
      },
      result: null
    }
  },
  methods: {
    success(data){
      this.result = data;
    }
  }
}
</script>

```
