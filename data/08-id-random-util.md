---
title: ID与随机数工具
classes:
  - cn.hutool.core.util.IdUtil
  - cn.hutool.core.util.RandomUtil
since: 3.0.0
tags: [ID, UUID, 雪花, snowflake, 随机数, random, IdUtil, RandomUtil]
---

# ID与随机数工具 — IdUtil / RandomUtil

## 概述

- **`IdUtil`**：唯一ID生成工具，支持 UUID、ObjectId、NanoId、Snowflake（雪花算法）等多种策略。
- **`RandomUtil`**：随机数工具，提供随机整数、字符串、集合元素选取、带权重随机等功能。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### IdUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `randomUUID()` | 标准 UUID（带中划线） | `String` | 3.0+ |
| `simpleUUID()` | 简单 UUID（不带中划线） | `String` | 3.0+ |
| `fastUUID()` | 高性能 UUID（不用 SecureRandom） | `String` | 5.0+ |
| `fastSimpleUUID()` | 高性能简单 UUID | `String` | 5.0+ |
| `objectId()` | MongoDB 风格 ObjectId | `String` | 4.0+ |
| `nanoId()` | NanoID（21位） | `String` | 5.6+ |
| `nanoId(size)` | 指定长度的 NanoID | `String` | 5.6+ |
| `getSnowflake(workerId, datacenterId)` | 获取雪花算法实例 | `Snowflake` | 4.0+ |
| `getSnowflakeNextId()` | 获取雪花 ID（long） | `long` | 5.7+ |
| `getSnowflakeNextIdStr()` | 获取雪花 ID（String） | `String` | 5.7+ |
| `createSnowflake(workerId, datacenterId)` | 创建新雪花算法实例 | `Snowflake` | 4.0+ |

### RandomUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `randomInt()` | 随机 int | `int` | 3.0+ |
| `randomInt(min, max)` | 范围随机 int（含min不含max） | `int` | 3.0+ |
| `randomInt(max)` | [0, max) 随机 int | `int` | 5.4+ |
| `randomLong()` | 随机 long | `long` | 3.0+ |
| `randomLong(min, max)` | 范围随机 long | `long` | 5.4+ |
| `randomDouble()` | [0, 1) 随机 double | `double` | 3.0+ |
| `randomDouble(min, max)` | 范围随机 double | `double` | 5.4+ |
| `randomBoolean()` | 随机布尔 | `boolean` | 3.0+ |
| `randomString(length)` | 随机字符串（字母+数字） | `String` | 3.0+ |
| `randomStringUpper(length)` | 随机大写字符串 | `String` | 3.0+ |
| `randomStringWithoutStr(length, excludeStr)` | 排除指定字符 | `String` | 5.4+ |
| `randomNumbers(length)` | 随机数字字符串 | `String` | 3.0+ |
| `randomEle(collection)` | 随机选取集合中一个元素 | `T` | 3.0+ |
| `randomEles(collection, count)` | 随机选取多个元素 | `List<T>` | 3.0+ |
| `randomEle(array)` | 随机选取数组中一个 | `T` | 5.4+ |
| `randomBytes(length)` | 随机字节数组 | `byte[]` | 3.0+ |
| `weightRandom(weightObjs)` | 带权重的随机 | `WeightRandom<T>` | 5.4+ |

## 详细 API 与示例

### UUID 生成

```java
import cn.hutool.core.util.IdUtil;

// 标准 UUID：550e8400-e29b-41d4-a716-446655440000
String uuid = IdUtil.randomUUID();

// 简单 UUID：550e8400e29b41d4a716446655440000（无中划线，32位）
String simpleUuid = IdUtil.simpleUUID();

// 高性能 UUID（使用 ThreadLocalRandom，性能更高但安全性略低）
String fastUuid = IdUtil.fastUUID();
String fastSimple = IdUtil.fastSimpleUUID();
```

> 💡 **UUID 变体对比**：
> | 变体 | 性能 | 安全性 | 长度 | 适用场景 |
> |------|------|--------|------|---------|
> | `randomUUID` | 一般 | 高（SecureRandom） | 36 | 安全要求高的场景 |
> | `simpleUUID` | 一般 | 高 | 32 | 数据库主键（无中划线） |
> | `fastUUID` | 高 | 一般（ThreadLocalRandom） | 36 | 高并发非安全场景 |
> | `fastSimpleUUID` | 高 | 一般 | 32 | 高并发日志 traceId |

### ObjectId / NanoId

```java
import cn.hutool.core.util.IdUtil;

// MongoDB 风格 ObjectId（24位十六进制）
String objectId = IdUtil.objectId();
// => "5f3e4a7b8c9d1e2f3a4b5c6d"

// NanoId（默认21位，URL 安全字符）
String nanoId = IdUtil.nanoId();
// => "V1StGXR8_Z5jdHi6B-myT"

// 指定长度
String shortId = IdUtil.nanoId(10);
// => "IRFa-VaY2b"
```

### 雪花算法 — Snowflake

```java
import cn.hutool.core.util.IdUtil;
import cn.hutool.core.lang.Snowflake;

// 简单用法（使用默认 workerId=0, datacenterId=0）
long id = IdUtil.getSnowflakeNextId();        // 1234567890123456789
String idStr = IdUtil.getSnowflakeNextIdStr(); // "1234567890123456789"

// 自定义 workerId 和 datacenterId
Snowflake snowflake = IdUtil.getSnowflake(1, 1);
long id2 = snowflake.nextId();
String id2Str = snowflake.nextIdStr();

// 创建新实例（不使用缓存）
Snowflake sf = IdUtil.createSnowflake(2, 3);
```

> ⚠️ **分布式环境注意事项**：
> 1. `workerId` 范围 0~31，`datacenterId` 范围 0~31。
> 2. **不同节点必须使用不同的 workerId**，否则可能生成重复 ID。
> 3. 常见配置方式：通过环境变量、配置中心、或基于 IP 地址末段计算。
> 4. `getSnowflake` 使用单例缓存，同参数返回同一实例；`createSnowflake` 每次新建。

### RandomUtil — 随机数

```java
import cn.hutool.core.util.RandomUtil;

// 随机整数
int n1 = RandomUtil.randomInt();          // 任意 int
int n2 = RandomUtil.randomInt(100);       // [0, 100)
int n3 = RandomUtil.randomInt(10, 20);    // [10, 20)

// 随机长整数
long l = RandomUtil.randomLong(1000, 9999);

// 随机浮点数
double d = RandomUtil.randomDouble();          // [0, 1)
double d2 = RandomUtil.randomDouble(0, 100);   // [0, 100)

// 随机布尔
boolean b = RandomUtil.randomBoolean();
```

### RandomUtil — 随机字符串

```java
import cn.hutool.core.util.RandomUtil;

// 随机字母+数字字符串
String s1 = RandomUtil.randomString(8);          // "a3Bf9xKm"

// 随机大写字符串
String s2 = RandomUtil.randomStringUpper(6);     // "X8K2MN"

// 纯数字字符串（验证码常用）
String code = RandomUtil.randomNumbers(6);       // "382957"

// 排除易混淆字符（如 0/O, 1/I/l）
String safe = RandomUtil.randomStringWithoutStr(8, "0OlI1");
```

### RandomUtil — 集合随机选取

```java
import cn.hutool.core.util.RandomUtil;
import cn.hutool.core.collection.CollUtil;

List<String> colors = CollUtil.newArrayList("红", "橙", "黄", "绿", "蓝");

// 随机选一个
String one = RandomUtil.randomEle(colors);       // "绿"

// 随机选多个（可重复）
List<String> some = RandomUtil.randomEles(colors, 3);
// => ["蓝", "红", "蓝"]

// 从数组随机选
String[] arr = {"A", "B", "C"};
String picked = RandomUtil.randomEle(arr);
```

### RandomUtil — 带权重随机

```java
import cn.hutool.core.util.RandomUtil;
import cn.hutool.core.lang.WeightRandom;

// 带权重的随机选择（如抽奖）
WeightRandom<String> wr = RandomUtil.weightRandom(
    new WeightRandom.WeightObj<>("一等奖", 1),    // 权重1
    new WeightRandom.WeightObj<>("二等奖", 5),    // 权重5
    new WeightRandom.WeightObj<>("三等奖", 20),   // 权重20
    new WeightRandom.WeightObj<>("谢谢参与", 74)  // 权重74
);

String prize = wr.next();  // 按权重随机
```

## 常见问题 FAQ

### Q: UUID、ObjectId、NanoId、Snowflake 怎么选？
**A**:
| 方案 | 有序性 | 长度 | 性能 | 适用场景 |
|------|--------|------|------|---------|
| UUID | 无序 | 32/36 字符 | 高 | 通用唯一标识 |
| ObjectId | 时间有序 | 24 字符 | 高 | MongoDB 风格 |
| NanoId | 无序 | 可配置 | 高 | URL 安全短 ID |
| Snowflake | 时间有序 | 19 位数字 | 极高 | **分布式系统主键（推荐）** |

### Q: Snowflake 的时钟回拨问题？
**A**: Hutool 的 Snowflake 默认会等待时钟追上，但极端情况下可能阻塞。生产环境建议配合 NTP 同步使用。

### Q: RandomUtil 线程安全吗？
**A**: 是的，底层使用 `ThreadLocalRandom`，线程安全且高性能。

### Q: randomString 生成的字符包含哪些？
**A**: 小写字母 a-z + 数字 0-9。`randomStringUpper` 为大写 A-Z + 数字 0-9。

## 最佳实践

1. **数据库主键用 Snowflake**：有序、高性能、分布式安全。
2. **高并发 traceId 用 `fastSimpleUUID`**：性能优于 `randomUUID`。
3. **验证码用 `randomNumbers`**：纯数字，用户体验好。
4. **排除易混淆字符**：`randomStringWithoutStr(8, "0OlI1")` 避免人工抄写错误。
5. **抽奖用 `weightRandom`**：权重控制概率，比手写随机逻辑可靠。
6. **同一应用内 Snowflake 用 `getSnowflake`**：单例模式，避免重复创建。
