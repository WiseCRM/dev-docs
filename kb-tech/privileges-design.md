# WiseCRM365 权限设计

## 概述

WiseCRM365 权限管理是以 [RBAC](https://baike.baidu.com/item/%E5%9F%BA%E4%BA%8E%E8%A7%92%E8%89%B2%E7%9A%84%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6/8795406)（基于角色的访问控制）为基础，并在此之上通过更多扩展设计而成。同时支持按组织结构（部门）分层级的权限划分，让上下级部门之间可以更好的共享数据。除此以外还扩展了基于条件的权限控制，让权限粒度可以细微到具体某一个字段的值。



## 设计实现

在 WiseCRM365 中，有 3 张核心表来构成整个权限体系。

* User 用户
* BusinessUnit 部门 - 层级划分
* Role 角色 - 定义权限

用户被指定隶属于哪个部门及应用于哪个（哪些）角色，基于此，对用户所属角色或部门的修改会直接影响用户的权限范围。

### 部门层级

对用户的权限控制可以按照部门层级来划分，包括以下几个层级。

* 本人
* 本部门
* 本部门及下级部门
* 全部

这主要是通过业务表中的 `ownUser` 和 `ownBusinessUnit` 字段来实现（所属用户和所属部门），这两个字段会强制保持同步（所以在更改用户所在部门时，可能会触发更新所有业务表）。通过这两个字段的协调，可以很方便的实现用户层级权限控制，同时对于查询效率较为友好。

### 过滤条件

在更为复杂的业务场景中，上述基于部门分层的权限控制仍旧相对粗放，因此 WiseCRM365 扩展了基于条件的权限控制。即通过查询过滤器，可以在上述层级权限的基础之上做进一步的控制。

例如 *销售组A* 被授权查看来源于百度的客户，而 *销售组B* 可以查看来源于 Google 的。实现这一需求，只需要通过过滤器设定相应的过滤条件即可。

 
