+++
title = 'Restful API'
date = '2024-07-02'
author = 'aiclr'
categories = ['develop']
tags = ['restful','spring','web','java']
Summary='以资源为中心进行URL设计'
+++

## 以资源为中心进行URL设计

资源是 **RESTful API** 的核心，所有操作都是针对某一特定资源进行 \
简洁、清晰、结构化的 URL<sub>统一资源定位符</sub>设计至关重要

- `/users`
- `/users/{id}`
- `/users/{loginName}`
- `/products/{id}/comments`

根据 **RFC3986** 标准中规定，**URL** 是大小写敏感的，**SpringMVC** 默认对 **URL** 区分大小写，避免歧义定义URL时最好都使用小写字母。RESTful API 设计最好做到 Hypermedia 化，也就是在返回结果中包含所提供相关资源的链接使得用户可以根据返回结果就能得到后续操作需要访问的地址；这种设计也被称为 **HATEOAS**<sub>Hypermedia As The Engine Of Application State</sub>

例如 **github api** 设计：[https://api.github.com](https://api.github.com) 获取api列表
```json
   {
     "current_user_url": "https://api.github.com/user",
     "current_user_authorizations_html_url": "https://github.com/settings/connections/applications{/client_id}",
   }
```

## 正确使用HTTP方法及状态码

- 2xx：请求正常处理并返回
- 3xx：重定向，请求资源位置发生变化
- 4xx：客户端发送的请求有错误
- 5xx：服务器端错误
- 查询及分页处理原则

### 例子

- `xxx?state=closed`：查询指定状态
- `xxx?limit=10`：指定返回10条记录
- `xxx?page=2&size=25&sort=created,desc`：分页查询及排序方式

**spring Data** 的 **Pageable** 进行分页查询时，需要传入的分页参数的默认名如下
- page
- size
- sort<sub>ASC|DESC默认ASC</sub>
    - sort=firstname&sort=lastname,asc

如果分页参数和项目中不同，可以通过实现 ***PageableHandlerMethodArgument ResolverCustomizer*** 接口进行自定义

## 其他指导原则

- 使用 **JSON** 作为响应返回格式
- API 域名，尽量将API部署在一个专用域名下
- API 版本最好放到URL中 ***https://xxx/v1/xxx***
- 给出明确的错误信息