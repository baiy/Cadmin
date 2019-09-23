# 数据库

```sql
CREATE TABLE `admin_auth`
(
    `id`          int(10) unsigned NOT NULL AUTO_INCREMENT,
    `name`        varchar(100)     NOT NULL DEFAULT '',
    `description` varchar(255)     NOT NULL DEFAULT '',
    `create_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` timestamp        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    KEY `name` (`name`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='权限';

CREATE TABLE `admin_menu`
(
    `id`          int(10) unsigned NOT NULL AUTO_INCREMENT,
    `parent_id`   int(10) unsigned NOT NULL DEFAULT '1',
    `name`        varchar(100)     NOT NULL DEFAULT '',
    `url`         varchar(255)     NOT NULL DEFAULT '',
    `icon`        varchar(30)      NOT NULL DEFAULT '',
    `description` varchar(255)     NOT NULL DEFAULT '',
    `sort`        int(10) unsigned NOT NULL DEFAULT '0',
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
    `name`        varchar(100)        NOT NULL DEFAULT '',
    `action`      varchar(100)        NOT NULL DEFAULT '',
    `call`        varchar(100)        NOT NULL DEFAULT '',
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
    `username`        varchar(100)        NOT NULL DEFAULT '',
    `password`        varchar(255)        NOT NULL DEFAULT '',
    `last_login_ip`   varchar(15)         NOT NULL DEFAULT '',
    `last_login_time` timestamp           NULL     DEFAULT NULL,
    `status`          tinyint(1) unsigned NOT NULL DEFAULT '1',
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
    `name`        varchar(100)     NOT NULL DEFAULT '',
    `description` varchar(255)     NOT NULL DEFAULT '',
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
    `token`         varchar(32)         NOT NULL DEFAULT '',
    `admin_user_id` int(10) unsigned    NOT NULL,
    `expire_time`   timestamp           NULL     DEFAULT NULL,
    `create_time`   timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`   timestamp           NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    PRIMARY KEY (`id`) USING BTREE,
    UNIQUE KEY `token` (`token`) USING BTREE,
    KEY `expire_time` (`expire_time`) USING BTREE
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4 COMMENT ='登录token';
```