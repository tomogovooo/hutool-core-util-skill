---
title: JDBC 与数据库工具
classes:
  - cn.hutool.db.Db
  - cn.hutool.db.Entity
  - cn.hutool.db.Session
  - cn.hutool.db.ds.DSFactory
module: hutool-db
verified: 5.8.47
tags: [DB, JDBC, SQL, Entity, 事务, Db]
---

# JDBC 与数据库工具

本文档适用于 Hutool 5.8.x。`hutool-db` 适合轻量 JDBC 辅助；项目已经使用 Spring JDBC、MyBatis、JPA 或统一数据访问层时，应服从其事务和映射边界。

## 目录

- [入口选择](#入口选择)
- [查询](#查询)
- [新增、修改与删除](#新增修改与删除)
- [事务](#事务)
- [SQL 与数据安全](#sql-与数据安全)
- [依赖和生命周期](#依赖和生命周期)

## 入口选择

| 场景 | API |
|---|---|
| 基于现有数据源执行 SQL | `Db.use(dataSource)` |
| 使用 Hutool 默认/分组数据源 | `Db.use()`、`Db.use(group)` |
| 表名和字段值载体 | `Entity` |
| 手动管理同一连接的事务 | `Session` 或 `Db.tx` |
| 数据源工厂 | `DSFactory` | 

优先把应用容器管理的 `DataSource` 传给 `Db.use(dataSource)`，不要在每次请求中创建连接池。

## 查询

```java
import cn.hutool.db.Db;
import cn.hutool.db.Entity;

import java.util.List;

Db db = Db.use(dataSource);
List<Entity> rows = db.query(
        "select id, name from sys_user where status = ? order by id",
        status
);

for (Entity row : rows) {
    Long id = row.getLong("id");
    String name = row.getStr("name");
}
```

单条查询可使用 `queryOne`，Bean 映射可使用带目标类型的查询重载。映射前确认列别名、大小写、null、枚举、时间和数值精度。

## 新增、修改与删除

```java
Entity record = Entity.create("sys_user")
        .set("name", name)
        .set("status", 1);

Long generatedId = db.insertForGeneratedKey(record);

Entity update = Entity.create("sys_user").set("status", 0);
Entity where = Entity.create("sys_user").set("id", generatedId);
db.update(update, where);

db.del(Entity.create("sys_user").set("id", generatedId));
```

`Entity` 是通用字段容器，不替代领域校验。执行 update/delete 前显式检查条件，防止空条件或过宽条件影响整表；关键写操作记录业务审计信息。

## 事务

```java
Db.use(dataSource).tx(db -> {
    db.insert(Entity.create("orders")
            .set("order_no", orderNo)
            .set("status", "CREATED"));

    db.update(
            Entity.create("inventory").set("available", newAvailable),
            Entity.create("inventory").set("sku", sku)
    );
});
```

只有在 Hutool 自己管理连接时才使用这类事务入口。Spring `@Transactional`、JTA 或框架 Session 已经建立事务时，不要再开启独立 Hutool 事务，否则可能使用不同连接、出现部分提交。

## SQL 与数据安全

- 值使用 `?` 占位符和参数绑定，不拼接用户输入。
- JDBC 参数不能代替表名、列名和排序方向；这些动态标识符必须来自服务器端白名单。
- 分页查询必须有稳定排序，并限制最大 page size；不要把无界查询全部加载进内存。
- 避免 `select *`，明确所需列。敏感列按最小权限查询并在日志中脱敏。
- 检查受影响行数；并发更新需要版本号、条件更新或数据库锁策略，不能只靠先查后改。
- 数据库异常不要直接返回给调用方；保留可追踪信息，同时隐藏 SQL、连接串和敏感参数。
- 使用数据库方言支持的语法，跨数据库代码不要假设自动兼容。

## 依赖和生命周期

所属模块为 `cn.hutool:hutool-db`。数据库驱动和连接池通常由应用显式提供；`hutool-db` 不等于自动拥有可运行的数据源。先检查 JDBC 驱动、连接池、项目 BOM 和部署环境。

连接、Statement 和 ResultSet 的关闭通常由 Hutool 调用封装处理，但调用方创建的流、DataSource 和应用级连接池仍由其所有者管理。不要在单次 DAO 调用后关闭共享 DataSource。

适合小型 JDBC 操作、脚本和已有轻量数据层；复杂对象关系、动态 SQL、迁移、审计、多租户和框架事务场景优先现有数据访问框架。

官方 API：[Db](https://plus.hutool.cn/apidocs/cn/hutool/db/Db.html)、[AbstractDb](https://plus.hutool.cn/apidocs/cn/hutool/db/AbstractDb.html)、[Entity](https://plus.hutool.cn/apidocs/cn/hutool/db/Entity.html)。
