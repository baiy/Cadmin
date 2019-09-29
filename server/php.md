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
数据库结构详见 [数据库结构](server/db.md) 一章

数据库数据详见 [数据库数据](server/php.md?id=数据库数据)

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



### 其他说明
#### 数据库数据
```sql
INSERT INTO `admin_auth`(`id`, `name`)
VALUES (1, '系统设置-用户管理'),
       (2, '系统设置-权限管理');

INSERT INTO `admin_menu`(`id`, `parent_id`, `name`, `url`, `icon`, `description`, `sort`)
VALUES (1, 0, '系统设置', '', 'md-settings', '', 1),
       (10, 1, '用户管理', '', 'md-contacts', '', 1),
       (11, 10, '用户', '/system/user', 'md-person', '', 1),
       (12, 10, '用户组', '/system/userGroup', 'md-people', '', 2),
       (20, 1, '权限管理', '', 'md-list', '', 2),
       (21, 20, '请求', '/system/request', 'md-list', '', 1),
       (22, 20, '菜单', '/system/menu', 'md-menu', '', 2),
       (23, 20, '权限', '/system/auth', 'md-lock', '', 4);

INSERT INTO `admin_menu_relate`(`admin_auth_id`, `admin_menu_id`)
VALUES (1, 1),
       (1, 10),
       (1, 11),
       (1, 12),
       (2, 20),
       (2, 21),
       (2, 22),
       (2, 23);

INSERT INTO `admin_request`(`id`, `type`, `name`, `action`, `call`)
VALUES (1, 'default', '登录', '/login', '\\Baiy\\Cadmin\\System\\Index::login'),
       (2, 'default', '退出', '/logout', '\\Baiy\\Cadmin\\System\\Index::logout'),
       (3, 'default', '初始数据加载', '/load', '\\Baiy\\Cadmin\\System\\Index::load'),
       (10, 'default', '用户管理-用户-列表数据', '/system/user/lists', '\\Baiy\\Cadmin\\System\\User::lists'),
       (11, 'default', '用户管理-用户-保存', '/system/user/save', '\\Baiy\\Cadmin\\System\\User::save'),
       (12, 'default', '用户管理-用户-删除', '/system/user/remove', '\\Baiy\\Cadmin\\System\\User::remove'),
       (20, 'default', '用户管理-用户组-列表数据', '/system/userGroup/lists', '\\Baiy\\Cadmin\\System\\UserGroup::lists'),
       (21, 'default', '用户管理-用户组-保存', '/system/userGroup/save', '\\Baiy\\Cadmin\\System\\UserGroup::save'),
       (22, 'default', '用户管理-用户组-删除', '/system/userGroup/remove', '\\Baiy\\Cadmin\\System\\UserGroup::remove'),
       (23, 'default', '用户管理-用户组-获取用户分组信息', '/system/userGroup/getUser', '\\Baiy\\Cadmin\\System\\UserGroup::getUser'),
       (24, 'default', '用户管理-用户组-用户分配', '/system/userGroup/assignUser', '\\Baiy\\Cadmin\\System\\UserGroup::assignUser'),
       (25, 'default', '用户管理-用户组-移除用户分配', '/system/userGroup/removeUser', '\\Baiy\\Cadmin\\System\\UserGroup::removeUser'),
       (30, 'default', '权限管理-请求-列表数据', '/system/request/lists', '\\Baiy\\Cadmin\\System\\Request::lists'),
       (31, 'default', '权限管理-请求-保存', '/system/request/save', '\\Baiy\\Cadmin\\System\\Request::save'),
       (32, 'default', '权限管理-请求-删除', '/system/request/remove', '\\Baiy\\Cadmin\\System\\Request::remove'),
       (33, 'default', '权限管理-请求-类型映射', '/system/request/type', '\\Baiy\\Cadmin\\System\\Request::type'),
       (40, 'default', '权限管理-菜单-列表数据', '/system/menu/lists', '\\Baiy\\Cadmin\\System\\Menu::lists'),
       (41, 'default', '权限管理-菜单-排序', '/system/menu/sort', '\\Baiy\\Cadmin\\System\\Menu::sort'),
       (42, 'default', '权限管理-菜单-保存', '/system/menu/save', '\\Baiy\\Cadmin\\System\\Menu::save'),
       (43, 'default', '权限管理-菜单-删除', '/system/menu/remove', '\\Baiy\\Cadmin\\System\\Menu::remove'),
       (60, 'default', '权限管理-权限-列表数据', '/system/auth/lists', '\\Baiy\\Cadmin\\System\\Auth::lists'),
       (61, 'default', '权限管理-权限-保存', '/system/auth/save', '\\Baiy\\Cadmin\\System\\Auth::save'),
       (62, 'default', '权限管理-权限-删除', '/system/auth/remove', '\\Baiy\\Cadmin\\System\\Auth::remove'),
       (63, 'default', '权限管理-权限-获取请求分配信息', '/system/auth/getRequest', '\\Baiy\\Cadmin\\System\\Auth::getRequest'),
       (64, 'default', '权限管理-权限-请求分配', '/system/auth/assignRequest', '\\Baiy\\Cadmin\\System\\Auth::assignRequest'),
       (65, 'default', '权限管理-权限-移除请求分配', '/system/auth/removeRequest', '\\Baiy\\Cadmin\\System\\Auth::removeRequest'),
       (70, 'default', '权限管理-权限-获取用户组分配信息', '/system/auth/getUserGroup', '\\Baiy\\Cadmin\\System\\Auth::getUserGroup'),
       (71, 'default', '权限管理-权限-用户组分配', '/system/auth/assignUserGroup', '\\Baiy\\Cadmin\\System\\Auth::assignUserGroup'),
       (72, 'default', '权限管理-权限-移除用户组分配', '/system/auth/removeUserGroup', '\\Baiy\\Cadmin\\System\\Auth::removeUserGroup'),
       (73, 'default', '权限管理-权限-获取菜单分配信息', '/system/auth/getMenu', '\\Baiy\\Cadmin\\System\\Auth::getMenu'),
       (74, 'default', '权限管理-权限-分配菜单', '/system/auth/assignMenu', '\\Baiy\\Cadmin\\System\\Auth::assignMenu');

INSERT INTO `admin_request_relate`(`admin_auth_id`, `admin_request_id`)
VALUES (1, 10),
       (1, 11),
       (1, 12),
       (1, 20),
       (1, 21),
       (1, 22),
       (1, 23),
       (1, 24),
       (1, 25),
       (2, 30),
       (2, 31),
       (2, 32),
       (2, 33),
       (2, 40),
       (2, 41),
       (2, 42),
       (2, 43),
       (2, 60),
       (2, 61),
       (2, 62),
       (2, 63),
       (2, 64),
       (2, 65),
       (2, 70),
       (2, 71),
       (2, 72),
       (2, 73),
       (2, 74);

INSERT INTO `admin_user`(`id`, `username`, `password`)
VALUES (1, 'admin', '$2y$10$ORNq20knqc0RgbIWZjKVkOOxNyGql0J/ztRhaumQkPMbkgWBbuSii');

INSERT INTO `admin_user_group`(`id`, `name`)
VALUES (1, '超级管理员');

INSERT INTO `admin_user_relate`(`admin_user_group_id`, `admin_user_id`)
VALUES (1, 1);

INSERT INTO `admin_user_group_relate`(`admin_auth_id`, `admin_user_group_id`)
VALUES (1, 1),(2, 1);
```


