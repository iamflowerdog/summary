## 2018年6月7日 CRM 服务内容可配置 项目

#### route

* `React-router` 提供了一些 `router` 的核心api，但是没有提供dom操作进行跳转的api；
* `React-router-dom` 提供了 `BrowserRouter`,`Route`,`Link`等api，开发多用；
* `Route` 是路由的一个原材料，它是控制路径对应显示的组件，我们经常使用的是exact、path、component

    * `exact`控制匹配到 `/`路径时不再继续向下匹配
    * `path`标识路由的路径
    * `component`表示路径对应显示的组件
    
  
#### antd 中 `getFieldDecorator(id, options)` 校验规则

**问题描述**

```
由于要在提交前，需要对表单内的各个 field 进行再次校验，使用了 validateFieldsAndScroll。但发现并未生效。
后来经调试，发现如果对该函数传入一组指定的 field，是可以生效的。但只要这组 fileds 中，
包含了任意一个在校验规则中指定使用 validator 辅助校验的，则整个 validateFields 方法都不生效。
最后，才发现，是因为，我一开始并没有在对应的 validator 回调中，
总是返回 callback() （觉得很多余，而且一开始不写也可以对单个 input 做正常校验）。加上以后就 ok 了。

```

```
handleConfirmPassword = (rule, value, callback) => {
        const { getFieldValue } = this.props.form
        if (value && value !== getFieldValue('newPassword')) {
            callback('两次输入不一致！')
        }

        // Note: 必须总是返回一个 callback，否则 validateFieldsAndScroll 无法响应
        callback()
 }
```

`render()`

```
<FormItem
    {...formItemLayout}
    label="确认密码"
>
{
    getFieldDecorator('confirmPassword', {
           rules: [{
                  required: true,
                  message: '请再次输入以确认新密码',
            }, {
                  validator: this.handleConfirmPassword
            }],
    })(<Input type="password" />)
}
</FormItem>

```

**正则匹配** `开头不能为0或不超过 9999999` 的数字 

  `/^[1-9]\d{0,6}$/`
  
**获取表单提交数据** 

```

form.validateFieldsAndScroll((err, value)=> {
    if(!err){
        let option = Object.assign({}, value)
    }
    
   ...
})

// 通过 Object.assign() 方法获取表单全部数据

```

#### 前端编辑 Table 列表数据


**修改列表数据**

```

handleEditItem(item){
        const serviceSingleDTOS = [...this.state.serviceSingleDTOS];
        // 先找到要修改list数组的索引
        let target = serviceSingleDTOS.findIndex((element) => {
            return element.id === item.id;
        });

        if(target || target === 0){
            serviceSingleDTOS[target] = item;
            this.setState({serviceSingleDTOS});
        }

    }

```


**为record添加key**

>  Warning: Each record in table should have a unique `key` prop,or set `rowKey` to an unique primary key.


## 2018年8月1日 CRM 商机-客户-订单 结构调整

#### 阅读需求文档

> 想清楚再做 需求分析能力

#### 数据刷新，分模块刷新

#### componentWillReceiveProps(nextProps)渲染时机问题

* 需要先判断一下是否有变化再刷新

```
componentWillReceiveProps(nextProps){
        if(nextProps.currentBoStageType !== this.state.currentBoStageType){
            this.setState({
                currentBoStageType: nextProps.currentBoStageType,
            }, () => {
                this.getTotalSchema('/schema/potential/bo-detailItem-schema')
            });
        }
    }



```
* `shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用`

#### ant-d `Checkbox`组件问题

* 更改 `defaultValue` 属性 不会触发页面重新渲染

```
约束性和非约束性组件

约束性组件，简单的说，就是由React管理了它的value，而非约束性组件的value就是原生的DOM管理的。 


```

* [defaultValue与value的区别 (react.js)](https://blog.csdn.net/function__/article/details/72357251)










