# 数据库

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
    `type`        tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '请求类型',
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