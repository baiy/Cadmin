### 内置组件
> 开发过程除了可以使用`[iviewui](https://www.iviewui.com)`的组件外, 系统还内置一些与后台开发常用的组件

#### 显示后台指定用户名称
```html
<username :id="1" default="未知用户" />
```
> `id`:用户ID 
>
>`default`:用户不存在显示文字

#### 权限检查
```html
<auth-check action="/system/auth/assignMenu">
    有权限是展示
    <span slot="without">无权限时展示</span>
</auth-check>
```
> `without` 插槽为可选
>
> 常用于根据用户指定请求(action)的权限判断结果展示不同的内容

#### 输入框式文件上传
```html
<upload-file v-model="url" action="/upload"></upload-file>
```

#### 列表页组件
```html
<table-lists></table-lists>
```
> 该组件集成筛选框/表格/分页等功能

#### 字段映射
```js
let map = [{v: 1, n: '启用'},{v: 2, n: '禁用'}]
```
```html
<field-map value="2" :map="map" valueField="v" descField=n"" />
```
> 以上代码输出:`禁用`
>
> 该组件常与映射字段的页面输出  `valueField`/`descField` 为可选
