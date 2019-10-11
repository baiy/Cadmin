# 数据库

## 常见问题

### 默认账号密码

> 默认后台账号密码: `admin` / `123456`

## 默认密码生成器密码生成策略

> `base64(sha256(sha256("用户密码"+"盐")+"盐")+"|"++"盐");`

## 数据库结构

```sql
CREATE TABLE `admin_auth`
(
    `id`          int(10) unsigned NOT NULL AUTO_INCREMENT,
    `name`        varchar(100)     NOT NULL DEFAULT '' COMMENT '名称',
    `description` varchar(255)     NOT NULL DEFAULT '' COMMENT '描述',
    `create_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    KEY `name` (`name`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='权限';

CREATE TABLE `admin_menu`
(
    `id`          int(10) unsigned NOT NULL AUTO_INCREMENT,
    `parent_id`   int(10) unsigned NOT NULL DEFAULT '0' COMMENT '上级菜单ID',
    `name`        varchar(100)     NOT NULL DEFAULT '' COMMENT '名称',
    `url`         varchar(255)     NOT NULL DEFAULT '' COMMENT '链接',
    `icon`        varchar(30)      NOT NULL DEFAULT '' COMMENT 'icon',
    `description` varchar(255)     NOT NULL DEFAULT '' COMMENT '描述',
    `sort`        int(10) unsigned NOT NULL DEFAULT '0' COMMENT '排序值 越小越在前',
    `create_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='菜单';

CREATE TABLE `admin_menu_relate`
(
    `id`                  int(10) unsigned NOT NULL AUTO_INCREMENT,
    `admin_auth_id`       int(10) unsigned NOT NULL,
    `admin_menu_id`       int(10) unsigned NOT NULL,
    `create_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `unique` (`admin_auth_id`, `admin_menu_id`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='菜单权限关联';

CREATE TABLE `admin_request`
(
    `id`          int(10) unsigned    NOT NULL AUTO_INCREMENT,
    `type`        varchar(50)         NOT NULL DEFAULT 'default' COMMENT '请求类型',
    `name`        varchar(100)        NOT NULL DEFAULT '' COMMENT '名称',
    `action`      varchar(100)        NOT NULL DEFAULT '' COMMENT 'action',
    `call`        varchar(100)        NOT NULL DEFAULT '' COMMENT '类型配置',
    `create_time` timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `action` (`action`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='请求';

CREATE TABLE `admin_request_relate`
(
    `id`                  int(10) unsigned NOT NULL AUTO_INCREMENT,
    `admin_auth_id` int(10) unsigned NOT NULL,
    `admin_request_id`    int(10) unsigned NOT NULL,
    `create_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `unique` (`admin_auth_id`, `admin_request_id`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='请求权限关联';

CREATE TABLE `admin_user`
(
    `id`              int(10) unsigned    NOT NULL AUTO_INCREMENT,
    `username`        varchar(100)        NOT NULL DEFAULT '' COMMENT '用户名',
    `password`        varchar(255)        NOT NULL DEFAULT '' COMMENT '密码',
    `last_login_ip`   varchar(15)         NOT NULL DEFAULT '' COMMENT '最后登录IP',
    `last_login_time` timestamp           NULL     DEFAULT NULL COMMENT '最后登录时间',
    `status`          tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '状态',
    `create_time`     timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`     timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `username` (`username`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='管理员用户';

CREATE TABLE `admin_user_relate`
(
    `id`                  int(10) unsigned NOT NULL AUTO_INCREMENT,
    `admin_user_group_id` int(10) unsigned NOT NULL,
    `admin_user_id`       int(10) unsigned NOT NULL,
    `create_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `unique` (`admin_user_group_id`, `admin_user_id`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='用户组关联';

CREATE TABLE `admin_user_group`
(
    `id`          int(10) unsigned NOT NULL AUTO_INCREMENT,
    `name`        varchar(100)     NOT NULL DEFAULT '' COMMENT '名称',
    `description` varchar(255)     NOT NULL DEFAULT '' COMMENT '描述',
    `create_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    KEY `name` (`name`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='用户组';

CREATE TABLE `admin_user_group_relate`
(
    `id`                  int(10) unsigned NOT NULL AUTO_INCREMENT,
    `admin_user_group_id` int(10) unsigned NOT NULL,
    `admin_auth_id` int(10) unsigned NOT NULL,
    `create_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`         timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `unique` (`admin_user_group_id`, `admin_auth_id`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='用户组权限关联';

CREATE TABLE `admin_token`
(
    `id`            bigint(20) unsigned NOT NULL AUTO_INCREMENT,
    `token`         varchar(32)         NOT NULL DEFAULT '' COMMENT 'token',
    `admin_user_id` int(10) unsigned    NOT NULL COMMENT '用户ID',
    `expire_time`   timestamp           NULL     DEFAULT NULL COMMENT '过期时间',
    `create_time`   timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`   timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `token` (`token`) USING BTREE,
    KEY `expire_time` (`expire_time`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='登录token';
```

## 数据库数据

```sql
INSERT INTO `admin_auth`(`id`, `name`)
VALUES (1, '系统设置-用户管理'),
       (2, '系统设置-权限管理');

INSERT INTO `admin_menu`(`id`, `parent_id`, `name`, `url`, `icon`, `description`, `sort`)
VALUES (1, 0, '系统设置', '', '', '', 1),
       (10, 1, '用户管理', '', '', '', 1),
       (11, 10, '用户', '/system/user', '', '', 1),
       (12, 10, '用户组', '/system/userGroup', '', '', 2),
       (20, 1, '权限管理', '', 'md-list', '', 2),
       (21, 20, '请求', '/system/request', '', '', 1),
       (22, 20, '菜单', '/system/menu', '', '', 2),
       (23, 20, '权限', '/system/auth', '', '', 4);

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
VALUES (1, 'default', '登录', '/login', 'Baiy.Cadmin.System.Index.login'),
       (2, 'default', '退出', '/logout', 'Baiy.Cadmin.System.Index.logout'),
       (3, 'default', '初始数据加载', '/load', 'Baiy.Cadmin.System.Index.load'),
       (10, 'default', '用户管理-用户-列表数据', '/system/user/lists', 'Baiy.Cadmin.System.User.lists'),
       (11, 'default', '用户管理-用户-保存', '/system/user/save', 'Baiy.Cadmin.System.User.save'),
       (12, 'default', '用户管理-用户-删除', '/system/user/remove', 'Baiy.Cadmin.System.User.remove'),
       (20, 'default', '用户管理-用户组-列表数据', '/system/userGroup/lists', 'Baiy.Cadmin.System.UserGroup.lists'),
       (21, 'default', '用户管理-用户组-保存', '/system/userGroup/save', 'Baiy.Cadmin.System.UserGroup.save'),
       (22, 'default', '用户管理-用户组-删除', '/system/userGroup/remove', 'Baiy.Cadmin.System.UserGroup.remove'),
       (23, 'default', '用户管理-用户组-获取用户分组信息', '/system/userGroup/getUser', 'Baiy.Cadmin.System.UserGroup.getUser'),
       (24, 'default', '用户管理-用户组-用户分配', '/system/userGroup/assignUser', 'Baiy.Cadmin.System.UserGroup.assignUser'),
       (25, 'default', '用户管理-用户组-移除用户分配', '/system/userGroup/removeUser', 'Baiy.Cadmin.System.UserGroup.removeUser'),
       (30, 'default', '权限管理-请求-列表数据', '/system/request/lists', 'Baiy.Cadmin.System.Request.lists'),
       (31, 'default', '权限管理-请求-保存', '/system/request/save', 'Baiy.Cadmin.System.Request.save'),
       (32, 'default', '权限管理-请求-删除', '/system/request/remove', 'Baiy.Cadmin.System.Request.remove'),
       (33, 'default', '权限管理-请求-类型映射', '/system/request/type', 'Baiy.Cadmin.System.Request.type'),
       (40, 'default', '权限管理-菜单-列表数据', '/system/menu/lists', 'Baiy.Cadmin.System.Menu.lists'),
       (41, 'default', '权限管理-菜单-排序', '/system/menu/sort', 'Baiy.Cadmin.System.Menu.sort'),
       (42, 'default', '权限管理-菜单-保存', '/system/menu/save', 'Baiy.Cadmin.System.Menu.save'),
       (43, 'default', '权限管理-菜单-删除', '/system/menu/remove', 'Baiy.Cadmin.System.Menu.remove'),
       (60, 'default', '权限管理-权限-列表数据', '/system/auth/lists', 'Baiy.Cadmin.System.Auth.lists'),
       (61, 'default', '权限管理-权限-保存', '/system/auth/save', 'Baiy.Cadmin.System.Auth.save'),
       (62, 'default', '权限管理-权限-删除', '/system/auth/remove', 'Baiy.Cadmin.System.Auth.remove'),
       (63, 'default', '权限管理-权限-获取请求分配信息', '/system/auth/getRequest', 'Baiy.Cadmin.System.Auth.getRequest'),
       (64, 'default', '权限管理-权限-请求分配', '/system/auth/assignRequest', 'Baiy.Cadmin.System.Auth.assignRequest'),
       (65, 'default', '权限管理-权限-移除请求分配', '/system/auth/removeRequest', 'Baiy.Cadmin.System.Auth.removeRequest'),
       (70, 'default', '权限管理-权限-获取用户组分配信息', '/system/auth/getUserGroup', 'Baiy.Cadmin.System.Auth.getUserGroup'),
       (71, 'default', '权限管理-权限-用户组分配', '/system/auth/assignUserGroup', 'Baiy.Cadmin.System.Auth.assignUserGroup'),
       (72, 'default', '权限管理-权限-移除用户组分配', '/system/auth/removeUserGroup', 'Baiy.Cadmin.System.Auth.removeUserGroup'),
       (73, 'default', '权限管理-权限-获取菜单分配信息', '/system/auth/getMenu', 'Baiy.Cadmin.System.Auth.getMenu'),
       (74, 'default', '权限管理-权限-分配菜单', '/system/auth/assignMenu', 'Baiy.Cadmin.System.Auth.assignMenu');

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
VALUES (1, 'admin', 'ZjU3NzE5ZWU3OWFlYjQ1MzMyNzI1NTI5NDNlNzZiZjk3ZGVlNWMwZDRkMTU1ZDRiOThlNWUwMjRmOGZlMmZmZnwxanVlYXEyOQ==');

INSERT INTO `admin_user_group`(`id`, `name`)
VALUES (1, '超级管理员');

INSERT INTO `admin_user_relate`(`admin_user_group_id`, `admin_user_id`)
VALUES (1, 1);

INSERT INTO `admin_user_group_relate`(`admin_auth_id`, `admin_user_group_id`)
VALUES (1, 1),(2, 1);
```