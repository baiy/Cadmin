Cadmin的服务端实现 php

> 项目地址: [https://github.com/baiy/Cadmin-service-php](https://github.com/baiy/Cadmin-service-php)

### 特点

使用适配器与外层系统进行对接, 减少外层系统的侵入. 内置`Thinkphp`/`Laravel`适配器, 对于其他php框架和系统可自行编写适配器.

### 安装

```
composer require baiy/cadmin
```

### 数据库

#### 数据库结构导入

详见 [数据库结构](server/db.md) 一章

> 默认后台账号密码: `admin` / `123456`

### 适配器
为便于开发和减少对外层系统的侵入, 后台系统并不强依赖外层系统的数据库操作/请求处理/响应处理等基础组件, 而是使用适配器形式对外暴露相关组件设置方法.

例如: 数据库操作组件后台系统使用的是[Medoo](https://medoo.in), 外部系统只需通过调用适配器的相关方法传入`PDO`对象即可

适配器主要提供如下功能的适配:
1. 获取外层系统PDO对象
1. 后台入口路由
1. 业务方法执行
1. 相应输出

#### 适配器开发
> 可以[点击这里](https://github.com/baiy/Cadmin-service-php/tree/master/src/Adapter) 查看系统内置的适配器代码

### 使用方法
> 在代码安装和数据库导入完毕后, 接下来需要将后台系统的入口代码嵌入当前系统的合适位置, 并进行相应的配置, 下面提供`Thinkphp`/`Laravel`框架具体使用方法

#### Thinkphp 6.0
> 代码插入位置: `/route/app.php`
 
```php
<?php
$admin = \Baiy\Cadmin\Admin::instance();
$admin->setAdapter(new \Baiy\Cadmin\Adapter\Think60\Adapter());
//$admin->setDebug(config('app.debug')); // 系统调试标示[可选]
//$admin->addNoCheckLoginRequestId($id); // 无需校验权限的api[可选]
//$admin->addOnlyLoginRequestId($id); // 只需登录即可访问的api[可选]
//$admin->setLogCallback(function(array $content){}); // 请求日志记录回调函数[可选]
//$admin->setDbConnection($name); // 数据库连接标示[可选]
$admin->router('/api/'); // api入口路由注册 请求记住此入口url
```


