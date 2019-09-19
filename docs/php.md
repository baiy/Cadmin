### 特点
1. 内置用户/权限/菜单等基础功能
1. 使用适配器与外层系统进行对接, 减少外层系统的侵入. 内置`Thinkphp`/`Laravel`适配器, 对于其他php框架和系统可自行编写适配器.

> 项目地址: [https://github.com/baiy/Cadmin-service-php](https://github.com/baiy/Cadmin-service-php)

### 安装
```
composer require baiy/cadmin
```

### 数据库

#### 数据库结构导入
详见 [数据库结构](db.md) 一章

#### 数据库数据初始化
```sql
INSERT INTO `admin_group`(`id`, `name`) VALUES (1, '系统管理员');
INSERT INTO `admin_user_group`(`id`, `admin_group_id`, `admin_user_id`) VALUES (3, 1, 1);
INSERT INTO `admin_menu`(`id`, `parent_id`, `name`, `url`, `icon`, `description`, `sort`) VALUES (1, 0, '系统设置', '', 'md-settings', '', 0),(2, 1, '管理员设置', '/system/user', 'ios-people', '', 0),(3, 1, '权限管理', '', 'ios-list', '', 1),(4, 3, '请求管理', '/system/request', 'ios-list', '', 1),(5, 3, '权限组', '/system/group', 'ios-lock-outline', '', 0),(6, 3, '菜单管理', '/system/menu', 'md-menu', '', 2);
INSERT INTO `admin_menu_group`(`id`, `admin_group_id`, `admin_menu_id`) VALUES (1, 1, 1),(2, 1, 2),(3, 1, 3),(4, 1, 4),(5, 1, 5),(7, 1, 6),(20, 2, 1),(25, 2, 2),(26, 2, 3),(27, 2, 4),(28, 2, 5),(29, 2, 6);
INSERT INTO `admin_user`(`id`, `username`, `password`, `last_login_ip`, `last_login_time`, `status`) VALUES (1, 'admin', '$2y$10$ORNq20knqc0RgbIWZjKVkOOxNyGql0J/ztRhaumQkPMbkgWBbuSii', '127.0.0.1', '2000-10-10 10:10:10', 1);
INSERT INTO `admin_request`(`id`, `type`, `name`, `action`, `call`) VALUES (1, 1, '登录', '/login', '\\Baiy\\Cadmin\\System\\Index::login'),(2, 1, '退出', '/logout', '\\Baiy\\Cadmin\\System\\Index::logout'),(3, 1, '初始数据加载', '/load', '\\Baiy\\Cadmin\\System\\Index::load'),(100, 1, '菜单管理-列表数据', '/system/menu/lists', '\\Baiy\\Cadmin\\System\\Menu::lists'),(101, 1, '菜单管理-排序', '/system/menu/sort', '\\Baiy\\Cadmin\\System\\Menu::sort'),(102, 1, '菜单管理-菜单保存', '/system/menu/save', '\\Baiy\\Cadmin\\System\\Menu::save'),(103, 1, '菜单管理-菜单删除', '/system/menu/remove', '\\Baiy\\Cadmin\\System\\Menu::remove'),(104, 1, '请求管理-请求保存', '/system/request/save', '\\Baiy\\Cadmin\\System\\Request::save'),(105, 1, '请求管理-请求删除', '/system/request/remove', '\\Baiy\\Cadmin\\System\\Request::remove'),(106, 1, '请求管理-列表数据', '/system/request/lists', '\\Baiy\\Cadmin\\System\\Request::lists'),(107, 1, '管理员设置-列表数据', '/system/user/lists', '\\Baiy\\Cadmin\\System\\User::lists'),(108, 1, '管理员设置-用户保存', '/system/user/save', '\\Baiy\\Cadmin\\System\\User::save'),(109, 1, '管理员设置-用户删除', '/system/user/remove', '\\Baiy\\Cadmin\\System\\User::remove'),(110, 1, '管理员设置-获取用户分组', '/system/auth/getGroupByUser', '\\Baiy\\Cadmin\\System\\Auth::getGroupByUser'),(111, 1, '管理员设置-关联分组', '/system/auth/relateGroupToUser', '\\Baiy\\Cadmin\\System\\Auth::relateGroupToUser'),(112, 1, '权限管理-列表数据', '/system/group/lists', '\\Baiy\\Cadmin\\System\\Group::lists'),(113, 1, '权限管理-分组保存', '/system/group/save', '\\Baiy\\Cadmin\\System\\Group::save'),(114, 1, '权限管理-分组删除', '/system/group/remove', '\\Baiy\\Cadmin\\System\\Group::remove'),(115, 1, '权限管理-获取分配请求数据', '/system/auth/getRequest', '\\Baiy\\Cadmin\\System\\Auth::getRequest'),(116, 1, '权限管理-分配请求', '/system/auth/assignRequest', '\\Baiy\\Cadmin\\System\\Auth::assignRequest'),(117, 1, '权限管理-移除分配请求', '/system/auth/removeRequest', '\\Baiy\\Cadmin\\System\\Auth::removeRequest'),(118, 1, '权限管理-获取菜单分配数据', '/system/auth/getMenu', '\\Baiy\\Cadmin\\System\\Auth::getMenu'),(119, 1, '权限管理-分配菜单', '/system/auth/assignMenu', '\\Baiy\\Cadmin\\System\\Auth::assignMenu'),(120, 1, '权限管理-获取关联用户数据', '/system/auth/getUserByGroup', '\\Baiy\\Cadmin\\System\\Auth::getUserByGroup');
INSERT INTO `admin_request_group`(`id`, `admin_group_id`, `admin_request_id`) VALUES (1, 1, 100),(2, 1, 101),(3, 1, 102),(4, 1, 103),(5, 1, 104),(6, 1, 105),(7, 1, 106),(8, 1, 107),(9, 1, 108),(10, 1, 109),(11, 1, 110),(12, 1, 111),(13, 1, 112),(14, 1, 113),(15, 1, 114),(16, 1, 115),(17, 1, 116),(18, 1, 117),(19, 1, 118),(20, 1, 119),(22, 1, 120),(24, 2, 118),(25, 2, 117),(26, 2, 116),(27, 2, 120);
```
> 默认后台账号密码: `admin` / `123456`

### 适配器
为便于开发和减少对外层系统的侵入, 后台系统并不强依赖外层系统的数据库操作/请求处理/响应处理等基础组件, 而是使用适配器形式对外暴露相关组件设置方法.

例如: 数据库操作组件后台系统使用的是[Medoo](https://medoo.in), 外部系统只需通过调用适配器的相关方法传入`PDO`对象即可

适配器主要提供如下功能的适配:
1. 获取外层系统PDO对象
1. 后台入口路由
1. 业务方法执行
1. 相应输出

#### 适配器
> 可以[点击这里](https://github.com/baiy/Cadmin-service-php/tree/master/src/Adapter) 查看系统内置的适配器代码

### 使用方法
> 在代码安装和数据库导入完毕后, 接下来需要将后台系统的入口代码嵌入当前系统的合适位置, 并进行相应的配置, 下面提供`Thinkphp`/`Laravel`框架具体使用方法

#### Thinkphp 6.0
> 代码插入位置: `/route/app.php`
 
```php
$handle = \Baiy\Cadmin\Handle::instance();
$handle->setAdapter(new \Baiy\Cadmin\Adapter\Think60\Adapter());
//$handle->setDebug(config('app.debug')); // 系统调试标示[可选]
//$handle->addNoCheckLoginRequestId($id); // 无需校验权限的api[可选]
//$handle->addOnlyLoginRequestId($id); // 只需登录即可访问的api[可选]
//$handle->setLogCallback(function(array $content){}); // 请求日志记录回调函数[可选]
//$handle->setDbConnection($name); // 数据库连接标示[可选]
$handle->router('/api/'); // api入口路由注册 请求记住此入口url
```



### 其他问题
#### 请求日志数据结构


