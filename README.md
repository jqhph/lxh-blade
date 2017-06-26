# lxh-blade
轻量级js模板引擎

压缩后仅有5.1kb

## demo

```html
<div>
    @foreach {list} {row}
      @if {row.age} == 18
        <h4><span>18岁</span></h4>
      @elseif {row.age} == 19
        <h4><span>19岁</span></h4>
      @else
        <h4><span>{row.age}</span></h4>
      @endif

      <p><b>name: {row.name}</b></p>

      @foreach {row.attrs} {attr}
        <p><span>attrs: {attr}</span></p>
      @endforeach

      <p {@compare row.age > 19 ?? 'style="color:red"'} >
          {@compare row.age >= 20 ?? row.name :: 1}
      </p>
    @endforeach
</div>
```

```js
var vars = {
  list: [
    {name: '小猫', age: 18, attrs: {'花色', '淘气'}},
    {name: '小狗', age: 19, attrs: {'黑色', '活泼'}},
    {name: '小黄', age: 20, attrs: {'黄色', '爱哭'}}
  ]
}

// 第一个参数传入模板内容字符串，第二个参数传入模板变量键值对
var view = new Blade(tpl, vars)

// 渲染模板
// 第一个参数传入模板渲染后输出的容器class或id
// 第二个参数传入一个匿名函数，模板渲染成功后触发
view.render('.container', function (view) {
  console.log('模板渲染成功啦！')
})

// 3秒后重新渲染模板
setTimeout(function () {
  vars = {
    ...
  }
  view.rerander(vars)
}, 3000)

// 其余接口说明
/**
 * @param string|object 模板变量key，当传入的值为一个“object”类型时则会替换所有的键值对
 * @param mixed         模板变量值
 * @return void
 */
view.assign(key, value) 

/**
 * 获取编译后的模板内容
 * @param object vars 模板变量键值对
 * @return string 编译后的内容
 */ 
view.fetch(vars)

// 获取原模板内容
view.getTpl()
```

## 模板表达式

```js
// 普通变量
// 支持“vars.a”，“var[key]”，“var[key.a]”
{vars}

// 循环
// “{k}”可以省略，
@foreach {list} {k} {row}    // 必须换行
@endforeach                  // 换行或作为结束字符串

// 条件判断
@if ...                     // 必须换行
@elseif ...                 // 必须换行
@else                       // 必须换行
@endif                      // 换行或作为结束字符串

// 标签说明
// 三木运算，“:: value2”可省略
{@compare ... ?? value1 :: value2}

```


