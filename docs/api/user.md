### 用户

#### 列表

| action | 请求方式 |
| --- | --- |
| /system/user/lists | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| keyword | 否 |  筛选关键字 | 
| offset | 否 |  分页起始位置 | 
| pageSize | 否 |  分页大小 | 

##### 响应

| 参数 | 类型|描述 |
| --- | --- | --- |
| lists |array | 用户列表数据 |
| lists.group | array | 用户关联的用户组信息 |
| total | int | 列表总数 |

```json
{
    "lists": [
        {
            "id": 1,
            "username": "admin",
            "last_login_ip": "0.0.0.0",
            "last_login_time": "2019-09-23 18:36:02",
            "status": 1,
            "create_time": "2019-09-20 16:26:13",
            "update_time": "2019-09-23 18:36:02",
            "group": [
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

#### 添加/编辑用户

| action | 请求方式 |
| --- | --- |
| /system/user/save | POST |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | - |  用户ID 有值为编辑用户 否则为添加用户 | 
| username | - |  用户名 添加用户必填 | 
| password | - |  密码 添加用户必填 | 
| status | - |  状态 添加用户必填 |

#### 删除用户

| action | 请求方式 |
| --- | --- |
| /system/user/remove | GET |

##### 参数

| 参数名 | 必填 | 说明 |
| --- | --- | --- |
| id | 是 |  用户ID| 

### 用户组

#### 列表

#### 添加

#### 删除

#### 获取用户组用户信息

#### 添加用户到用户组

#### 移除用户从用户组