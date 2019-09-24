Cadmin的客户端实现 vue+iview

项目地址: <https://github.com/baiy/Cadmin-client-vue>

### 依赖
* [vue](https://cn.vuejs.org)
* [iview](https://www.iviewui.com)
* [localStorage](#)
* [axios](https://github.com/axios/axios)
* `vue-router` 
* `vuex`

### 安装

```shell
// 安装
npm install
// 开发
npm run serve
// 编译
npm run build
```

### 配置
配置文件地址:`./env`

| 配置变量名 | 说明| 
| --- | --- |
|VUE_APP_ADMIN_TOKEN_NAME|前端localStorage存储 `token` 名称|
|VUE_APP_API_URL_PREFIX|服务端数据请求入口地址|
|VUE_APP_API_ACTION_NAME|服务端数据请求地址中 `action` 变量名|
|VUE_APP_API_TOKEN_NAME|服务端数据请求地址中 `token` 变量名|
|VUE_APP_INDEX_URL|用户登录后模板现在页面|
|VUE_APP_SITE_NAME|站点名称 页面左上角|
|VUE_APP_SITE_TITLE_TPL|页面标题模板|

```js
import {config} from '/src/helper'
// 获取配置
let name = config('SITE_NAME')
```
> 获取配置时`VUE_APP_`无需填写

### 路由与菜单

##### 路由
系统中页面路由已经实现动态自动加载,加载规则如下:
1. 加载目录`/src/views`
1. 加载文件后缀名'.vue'
1. 会自动过滤`components`文件夹中的文件

例如:

| 文件路径 | 路由地址| 
| --- | --- |
|/src/views/system/user.vue|/system/user|
|/src/views/a/b/c/d.vue|/a/b/c/d|
|/src/views/a/components/c.vue|不自动加载路由|
|/src/views/components/a/c.vue|不自动加载路由|

##### 菜单
菜单分为两种类型:
1. 目录型: 点击该菜单会展示下级菜单,没有真实页面与之对应
1. 页面型: 这种菜单有前端页面对应,点击进入对应的前端页面
1. 链接型: http/https链接

页面型菜单: 后台配置菜单链接时对应这里 `路由地址`


### store
> `/src/store/admin.js`

| - | 路由地址| 
| --- | --- |
|adminUser|当前用户信息|
|adminMenu|用户已授权菜单列表|
|adminAllUser|后台所有用户列表|
|adminRequest|用户已授权请求列表|
|currentMenu|当前页面菜单信息|


### 服务端请求

#### vue 组件内部
```js
// get 请求
this.$request('/system/request/remove').
    data({id:1}).
    showSuccessTip().
    success(r=>{
         console.log(r)
    }).get();       
// post 请求
this.$request('/system/request/remove').
    data({id:1}).
    showSuccessTip().
    success(r=>{
         console.log(r)
    }).get();
```
`this.$request()` 方法参数为服务端请求`action`

`this.$request()` 方法返回值为 `/src/plugins/request.js` 中 `ActionRequest` 对象

发送请求时无需关心`token`,程序会自动附加

##### ActionRequest对象
| 方法 | 说明| 默认值|
| --- | --- |---|
|dataType(string)|响应格式|json|
|contentType(string)|请求头`Content-Type`|application/x-www-form-urlencoded|
|data(object)|请求数据对象|{}|
|showSuccessTip()|显示业务执行成功页面提示 不调用默认`不提示`||
|hideErrorTip()|隐藏业务执行异常页面提示 不调用默认`提示`||
|success(function)|业务执行成功回调函数|null|
|error(function)|业务执行异常回调函数|null|
|complete(function)|请求完成回调函数|null|
|get()|发起GET请求||
|post()|发起POST请求||

> 只有调用`get()`/`post()`方法才会发起请求


#### 任意位置
```js
import {request} from '/src/plugins/request'
import {actionUrl} from '/src/helper'

// 生成标准服务端action请求地址 已自动处理action/token参数
let url = actionUrl('action')

request({ 
    type:"get", // 请求方法
    data:{}, // 请求数据
    dataType:"json", // 响应格式
    contentType:"application/x-www-form-urlencoded",
    url:"", // 请求地址
    success, // 成功回调
    error, // 异常回调
    complete  // 完成回调
})
```
> 当然你也可以自己导入`axios` 按照`axios`方式发起请求

### 内置组件

开发过程除了可以使用[iviewui](https://www.iviewui.com)的组件外, 系统还内置一些与后台开发常用的组件

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
