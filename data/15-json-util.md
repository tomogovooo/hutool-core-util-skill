---
title: JSON 工具
classes:
  - cn.hutool.json.JSONUtil
  - cn.hutool.json.JSONObject
  - cn.hutool.json.JSONArray
  - cn.hutool.json.JSONConfig
module: hutool-json
verified: 5.8.47
tags: [JSON, 序列化, 反序列化, JSONPath, JSONUtil]
---

# JSON 工具

本文档适用于 Hutool 5.8.x。生成代码前先确认项目是否把 Jackson、Gson 或框架 `ObjectMapper` 作为统一序列化边界。

## 目录

- [核心入口](#核心入口)
- [解析与读取](#解析与读取)
- [Bean 与泛型转换](#bean-与泛型转换)
- [配置序列化](#配置序列化)
- [路径访问](#路径访问)
- [兼容性边界](#兼容性边界)

## 核心入口

| 需求 | API |
|---|---|
| 对象转 JSON 字符串 | `JSONUtil.toJsonStr`、`toJsonPrettyStr` |
| 字符串/对象转 JSON 对象 | `JSONUtil.parseObj` |
| 字符串/集合转 JSON 数组 | `JSONUtil.parseArray` |
| JSON 转 Bean | `JSONUtil.toBean` |
| JSON 数组转列表 | `JSONUtil.toList` |
| 读取或写入嵌套路径 | `JSONUtil.getByPath`、`putByPath` |

## 解析与读取

```java
import cn.hutool.json.JSONArray;
import cn.hutool.json.JSONObject;
import cn.hutool.json.JSONUtil;

JSONObject object = JSONUtil.parseObj(rawJson);
String name = object.getStr("name");
Integer age = object.getInt("age");

JSONArray items = object.getJSONArray("items");
JSONObject first = items.getJSONObject(0);

String compact = JSONUtil.toJsonStr(object);
String pretty = JSONUtil.toJsonPrettyStr(object);
```

不要只因输入“看起来像 JSON”就直接强转字段。对外部输入先检查对象/数组形态、必填字段、类型和业务范围，并按接口约定处理缺失值与 `null`。

## Bean 与泛型转换

```java
import cn.hutool.core.lang.TypeReference;
import cn.hutool.json.JSONUtil;

User user = JSONUtil.toBean(rawJson, User.class);

List<User> users = JSONUtil.toBean(
        rawArrayJson,
        new TypeReference<List<User>>() {},
        false
);
```

数组也可以使用：

```java
List<User> users = JSONUtil.toList(rawArrayJson, User.class);
```

涉及泛型嵌套时使用 `TypeReference`，不要先转成原始 `Map` 再做不安全强制转换。转换前核对构造方式、字段访问权限、枚举格式和日期格式。

## 配置序列化

```java
import cn.hutool.json.JSONConfig;
import cn.hutool.json.JSONUtil;

JSONConfig config = new JSONConfig()
        .setIgnoreNullValue(false)
        .setIgnoreCase(false)
        .setDateFormat("yyyy-MM-dd'T'HH:mm:ssXXX")
        .setStripTrailingZeros(false);

String json = JSONUtil.parseObj(value, config).toString();
```

常见配置包括忽略大小写、是否输出 null、日期格式、`transient` 字段支持和数字末尾零处理。`setDateFormat` 主要影响序列化，不应假定它自动定义所有 JSON 到 Bean 的日期解析规则。

## 路径访问

```java
Object city = JSONUtil.getByPath(object, "user.address.city");
JSONUtil.putByPath(object, "user.preferences.theme", "dark");
```

路径访问适合少量动态读取；字段固定且承载业务语义时，优先转换为明确的 DTO，获得类型检查和校验能力。

## 兼容性边界

- Hutool JSON 不是 Jackson/Gson 配置的无损替代品。迁移前比较字段命名、日期/时区、枚举、数字精度、未知字段、null 和自定义序列化器。
- 金额优先使用 `BigDecimal`，大整数不要经过 `double`；对 JavaScript 消费端还要考虑安全整数范围。
- 不要把任意外部 JSON 直接反序列化为可触发副作用、持久化或权限变更的领域对象；使用输入 DTO 并做校验。
- 控制输入大小和嵌套深度；不要在日志中输出令牌、密码或完整个人数据。
- Web 接口若由 Spring MVC/Jackson 管理，继续使用项目统一 `ObjectMapper`，避免响应格式在局部代码中漂移。
- 比较 JSON 语义时使用结构化比较，不要依赖对象字段顺序或格式化空白。

所属模块为 `cn.hutool:hutool-json`。只有 `hutool-core` 时不能使用上述类；新增依赖应与其他 Hutool 模块保持完全相同版本。

官方 API：[JSONUtil](https://plus.hutool.cn/apidocs/cn/hutool/json/JSONUtil.html)、[JSONConfig](https://plus.hutool.cn/apidocs/cn/hutool/json/JSONConfig.html)。
