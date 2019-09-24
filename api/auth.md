### 菜单

#### 全部菜单

| action | 请求方式 |
| --- | --- |
| /system/menu/lists | GET |

##### 响应

> 响应数据无需排序

```json
[
    {
        "id": 22,
        "parent_id": 20,
        "name": "菜单",
        "url": "\/system\/menu",
        "icon": "md-menu",
        "description": "",
        "sort": 0,
        "create_time": "2019-09-20 16:26:13",
        "update_time": "2019-09-24 16:58:07"
    },
    {
        "id": 1,
        "parent_id": 0,
        "name": "系统设置",
        "url": "",
        "icon": "md-settings",
        "description": "",
        "sort": 1,
        "create_time": "2019-09-20 16:26:13",
        "update_time": "2019-09-20 16:26:13"
    }
]
```

#### 添加/编辑菜单

| action | 请求方式 |
| --- | --- |
| /system/menu/save | POST |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | - |  菜单ID 有值为编辑 否则为添加 | 
| parent_id | 是 |  上级菜单ID 顶级菜单的parent_id为0 | 
| name | 是 |  菜单名称 | 
| url | 否 |  链接 | 
| icon | 否 |  菜单icon  | 
| description | 否 |  描述 | 

> url: 为空是菜单为`目录型`,可填写前端`路由地址`和`http链接`

#### 删除菜单

| action | 请求方式 |
| --- | --- |
| /system/menu/remove | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  菜单ID|

#### 排序

| action | 请求方式 |
| --- | --- |
| /system/menu/sort | POST |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| menus | 是 |  需要设置排序的菜单数组 |
| menus.id | 是 |  菜单ID|
| menus.sort | 是 |  菜单排序值|

```json
{
    "menus": [
        {
            "id": 10,
            "sort": 2
        },
        {
            "id": 13,
            "sort": 1
        },
        {
            "id": 23,
            "sort": 9
        }
    ]
}
```

### 请求(action)

#### 列表

| action | 请求方式 |
| --- | --- |
| /system/request/lists | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| keyword | 否 |  筛选关键字 | 
| offset | 否 |  分页起始位置 | 
| pageSize | 否 |  分页大小 | 

##### 响应

| 参数 | 类型|描述 |
| --- | --- | --- |
| lists |array | 请求列表 |
| lists.auth | array | 请求已关联的权限 |
| total | int | 列表总数 |

```json
{
    "lists": [
        {
            "id": 74,
            "type": 1,
            "name": "权限管理-权限组-分配菜单",
            "action": "\/system\/auth\/assignMenu",
            "call": "\\Baiy\\Cadmin\\System\\Auth::assignMenu",
            "create_time": "2019-09-20 16:26:13",
            "update_time": "2019-09-20 18:56:57",
            "auth": [
                {
                    "id": 2,
                    "name": "系统设置-权限管理",
                    "description": "",
                    "create_time": "2019-09-20 16:26:13",
                    "update_time": "2019-09-20 16:26:13"
                }
            ]
        },
        {
            "id": 73,
            "type": 1,
            "name": "权限管理-权限组-获取菜单分配信息",
            "action": "\/system\/auth\/getMenu",
            "call": "\\Baiy\\Cadmin\\System\\Auth::getMenu",
            "create_time": "2019-09-20 16:26:13",
            "update_time": "2019-09-20 16:26:13",
            "auth": [
                {
                    "id": 2,
                    "name": "系统设置-权限管理",
                    "description": "",
                    "create_time": "2019-09-20 16:26:13",
                    "update_time": "2019-09-20 16:26:13"
                }
            ]
        }
    ],
    "total": 30
}
```

#### 添加/编辑请求

| action | 请求方式 |
| --- | --- |
| /system/request/save | POST |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | - |  请求ID 有值为编辑 否则为添加 | 
| type | 是 |  请求类型 | 
| name | 是 |  名称 | 
| action | 是 |  action |
| call | 否 |  类型配置 |

#### 删除请求

| action | 请求方式 |
| --- | --- |
| /system/request/remove | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  请求ID|

### 权限

#### 列表

| action | 请求方式 |
| --- | --- |
| /system/auth/lists | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| keyword | 否 |  筛选关键字 | 
| offset | 否 |  分页起始位置 | 
| pageSize | 否 |  分页大小 | 

##### 响应

| 参数 | 类型|描述 |
| --- | --- | --- |
| lists |array | 权限列表 |
| lists.request | array | 权限已关联请求 |
| lists.menu | array | 权限已关联菜单 |
| lists.userGroup | array | 权限已关联用户组 |
| total | int | 列表总数 |

```json
{
    "lists": [
        {
            "id": 2,
            "name": "系统设置-权限管理",
            "description": "",
            "create_time": "2019-09-20 16:26:13",
            "update_time": "2019-09-20 16:26:13",
            "request": [
                {
                    "id": 30,
                    "type": 1,
                    "name": "权限管理-请求-列表数据",
                    "action": "\/system\/request\/lists",
                    "call": "\\Baiy\\Cadmin\\System\\Request::lists",
                    "create_time": "2019-09-20 16:26:13",
                    "update_time": "2019-09-20 16:26:13"
                }
            ],
            "menu": [
                {
                    "id": 20,
                    "parent_id": 1,
                    "name": "权限管理",
                    "url": "",
                    "icon": "md-list",
                    "description": "",
                    "sort": 2,
                    "create_time": "2019-09-20 16:26:13",
                    "update_time": "2019-09-20 16:26:13"
                }
            ],
            "userGroup": [
                {
                    "id": 1,
                    "name": "超级管理员",
                    "description": "123123123143423",
                    "create_time": "2019-09-20 16:26:13",
                    "update_time": "2019-09-20 17:42:41"
                }
            ]
        }
    ],
    "total": 2
}
```

#### 添加/编辑权限

| action | 请求方式 |
| --- | --- |
| /system/auth/save | POST |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | - |  权限ID 有值为编辑 否则为添加 | 
| name | 是 |  名称 | 
| description | 否 |  描述 |

#### 删除权限

| action | 请求方式 |
| --- | --- |
| /system/auth/remove | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID|

### 权限关联

#### 获取权限请求关联信息

| action | 请求方式 |
| --- | --- |
| /system/auth/getRequest | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 
| keyword | 否 |  未关联请求筛选关键字 | 
| offset | 否 |  分页起始位置 | 
| pageSize | 否 |  分页大小 | 

> 分页是相对于未关联请求而言的

##### 响应

| 参数 | 类型|描述 |
| --- | --- | --- |
| lists.assign |array | 已关联全部请求列表 |
| lists.noAssign | array | 未关联请求列表(分页) |
| total | int | 未关联请求总数 |

```json
{
    "lists": {
        "assign": [
            {
                "id": 74,
                "type": 1,
                "name": "权限管理-权限组-分配菜单",
                "action": "\/system\/auth\/assignMenu",
                "call": "\\Baiy\\Cadmin\\System\\Auth::assignMenu",
                "create_time": "2019-09-20 16:26:13",
                "update_time": "2019-09-20 18:56:57"
            }
        ],
        "noAssign": [
            {
                "id": 25,
                "type": 1,
                "name": "用户管理-用户组-移除用户分配",
                "action": "\/system\/userGroup\/removeUser",
                "call": "\\Baiy\\Cadmin\\System\\UserGroup::removeUser",
                "create_time": "2019-09-20 16:26:13",
                "update_time": "2019-09-20 16:26:13"
            }
        ]
    },
    "total": 9
}
```

#### 添加请求到权限

| action | 请求方式 |
| --- | --- |
| /system/auth/assignRequest | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 
| requestId | 是 |  请求ID | 

#### 移除请求从权限

| action | 请求方式 |
| --- | --- |
| /system/auth/removeRequest | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 
| requestId | 是 |  请求ID | 

#### 获取权限用户组关联信息

| action | 请求方式 |
| --- | --- |
| /system/auth/getUserGroup | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 
| keyword | 否 |  未关联用户组筛选关键字 | 
| offset | 否 |  分页起始位置 | 
| pageSize | 否 |  分页大小 | 

> 分页是相对于未关联用户组而言的

##### 响应

| 参数 | 类型|描述 |
| --- | --- | --- |
| lists.assign |array | 已关联全部用户组列表 |
| lists.noAssign | array | 未关联用户组列表(分页) |
| total | int | 未关联用户组总数 |

```json
{
    "lists": {
        "assign": [
            {
                "id": 1,
                "name": "超级管理员",
                "description": "123123123143423",
                "create_time": "2019-09-20 16:26:13",
                "update_time": "2019-09-20 17:42:41"
            }
        ],
        "noAssign": [
            {
                "id": 2,
                "name": "编辑",
                "description": "描述",
                "create_time": "2019-09-24 18:18:54",
                "update_time": "2019-09-24 18:18:54"
            }
        ]
    },
    "total": 1
}
```

#### 添加用户组到权限

| action | 请求方式 |
| --- | --- |
| /system/auth/assignUserGroup | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 
| userGroupId | 是 |  用户组ID | 

#### 移除用户组从权限

| action | 请求方式 |
| --- | --- |
| /system/auth/removeUserGroup | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 
| userGroupId | 是 |  用户组ID | 

#### 获取权限菜单关联信息

| action | 请求方式 |
| --- | --- |
| /system/auth/getMenu | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID | 

##### 响应

> 输出所有菜单的关联情况

| 参数 | 类型|描述 |
| --- | --- | --- |
| checked |bool | 是否已关联 |

```json
[
    {
        "id": 12,
        "parent_id": 10,
        "name": "用户组",
        "url": "\/system\/userGroup",
        "icon": "md-people",
        "description": "",
        "sort": 0,
        "create_time": "2019-09-20 16:26:13",
        "update_time": "2019-09-20 18:34:26",
        "checked": true
    }
]
```

#### 菜单关联权限

| action | 请求方式 |
| --- | --- |
| /system/auth/assignMenu | POST |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  权限ID |
| menuIds | 否 |  所有授权菜单ID数组 为空移除所有菜单授权  |

```json
{
    "id": 1,
    "menuIds": [10,12,20]
}
```
服务端更新逻辑有两种:
1. 先移除该权限的所有菜单授权,在添加对应的菜单授权 
1. 差异化更新, 获取数据库中已授权的菜单,通过diff算法对比出添加/删除的菜单,执行相关操作

