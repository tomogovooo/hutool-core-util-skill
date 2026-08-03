---
title: 正则表达式工具
classes:
  - cn.hutool.core.util.ReUtil
since: 3.0.0
tags: [正则, regex, 匹配, pattern, ReUtil]
---

# 正则表达式工具 — ReUtil

## 概述

`ReUtil` 封装了 `java.util.regex` 的常用操作，提供正则匹配、查找、提取、替换、删除等功能。同时 Hutool 内置了大量常用正则常量（`PatternPool`），可直接用于邮箱、手机号、身份证等校验。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `isMatch(pattern, content)` | 是否匹配正则 | `boolean` | 3.0+ |
| `get(pattern, content, groupIndex)` | 获取指定分组匹配 | `String` | 3.0+ |
| `getGroup0(pattern, content)` | 获取整体匹配（第0组） | `String` | 4.0+ |
| `getGroup1(pattern, content)` | 获取第1个分组 | `String` | 4.0+ |
| `findAll(pattern, content, group)` | 查找所有匹配 | `List<String>` | 3.0+ |
| `findAllGroup0(pattern, content)` | 查找所有整体匹配 | `List<String>` | 4.0+ |
| `findAllGroup1(pattern, content)` | 查找所有第1组匹配 | `List<String>` | 4.0+ |
| `extractMulti(pattern, content, template)` | 提取多个分组并格式化 | `String` | 3.0+ |
| `delFirst(pattern, content)` | 删除第一个匹配 | `String` | 3.0+ |
| `delAll(pattern, content)` | 删除所有匹配 | `String` | 4.0+ |
| `replaceAll(content, pattern, replacement)` | 全部替换 | `String` | 3.0+ |
| `escape(str)` | 转义正则特殊字符 | `String` | 3.0+ |
| `count(pattern, content)` | 匹配次数统计 | `int` | 5.0+ |
| `contains(pattern, content)` | 是否包含匹配 | `boolean` | 5.0+ |
| `getFirstNumber(content)` | 提取第一个数字 | `Integer` | 5.4+ |

## 详细 API 与示例

### isMatch — 正则匹配判断

```java
import cn.hutool.core.util.ReUtil;

// 是否匹配
boolean match = ReUtil.isMatch("\\d{4}-\\d{2}-\\d{2}", "2024-01-15");
// => true

boolean match2 = ReUtil.isMatch("[a-zA-Z]+", "Hello");
// => true

boolean match3 = ReUtil.isMatch("\\d+", "abc");
// => false
```

### get — 获取匹配分组

```java
import cn.hutool.core.util.ReUtil;

String content = "订单号：ORD-2024-001，金额：￥199.50";

// 获取整体匹配（第0组）
String order = ReUtil.getGroup0("ORD-\\d{4}-\\d{3}", content);
// => "ORD-2024-001"

// 获取第1个分组
String year = ReUtil.getGroup1("ORD-(\\d{4})-\\d{3}", content);
// => "2024"

// 获取指定分组
String amount = ReUtil.get("￥(\\d+\\.\\d{2})", content, 1);
// => "199.50"
```

### findAll — 查找所有匹配

```java
import cn.hutool.core.util.ReUtil;

String content = "联系电话：13812345678，备用：13987654321，固话：010-12345678";

// 查找所有手机号
List<String> phones = ReUtil.findAllGroup0("1[3-9]\\d{9}", content);
// => ["13812345678", "13987654321"]

// 查找所有数字
List<String> numbers = ReUtil.findAllGroup0("\\d+", content);
// => ["13812345678", "13987654321", "010", "12345678"]

// 查找所有分组（第1组）
String html = "<a href='url1'>text1</a><a href='url2'>text2</a>";
List<String> urls = ReUtil.findAllGroup1("href='(.*?)'", html);
// => ["url1", "url2"]
```

### extractMulti — 多分组提取

```java
import cn.hutool.core.util.ReUtil;

String content = "张三，年龄25，性别男";
String result = ReUtil.extractMulti(
    "(\\S+)，年龄(\\d+)，性别(\\S+)",
    content,
    "姓名：$1，年龄：$2岁，$3性"
);
// => "姓名：张三，年龄：25岁，男性"
```

### replaceAll / delFirst / delAll — 替换与删除

```java
import cn.hutool.core.util.ReUtil;

// 替换所有匹配
String result = ReUtil.replaceAll("Hello 123 World 456", "\\d+", "*");
// => "Hello * World *"

// 删除第一个匹配
String r2 = ReUtil.delFirst("\\d+", "abc123def456");
// => "abcdef456"

// 删除所有匹配
String r3 = ReUtil.delAll("\\d+", "abc123def456");
// => "abcdef"
```

### escape — 转义正则特殊字符

```java
import cn.hutool.core.util.ReUtil;

// 将字符串中的正则特殊字符转义
String escaped = ReUtil.escape("price is $3.50 (USD)");
// => "price is \\$3\\.50 \\(USD\\)"

// 安全地用用户输入构造正则
String userInput = "file.txt";
boolean match = ReUtil.isMatch(ReUtil.escape(userInput), "file.txt");
```

### count / contains — 统计与包含

```java
import cn.hutool.core.util.ReUtil;

// 统计匹配次数
int count = ReUtil.count("\\d+", "abc123def456ghi789");
// => 3

// 是否包含匹配（比 isMatch 更宽松，不要求完全匹配）
boolean has = ReUtil.contains("\\d+", "abc123");
// => true
```

### getFirstNumber — 提取第一个数字

```java
import cn.hutool.core.util.ReUtil;

Integer num = ReUtil.getFirstNumber("价格是99元，打8折");
// => 99
```

## 内置正则常量 — PatternPool

Hutool 内置了常用正则，通过 `PatternPool` 获取编译好的 `Pattern`，配合 `Validator` 使用：

```java
import cn.hutool.core.lang.PatternPool;
import cn.hutool.core.lang.Validator;

// 邮箱验证
Validator.isEmail("test@example.com");         // true

// 手机号验证
Validator.isMobile("13812345678");             // true

// 身份证号验证
Validator.isCitizenId("110101199001011234");    // true

// 中文验证
Validator.isChinese("你好世界");                // true

// URL 验证
Validator.isUrl("https://hutool.cn");          // true

// IPv4 验证
Validator.isIpv4("192.168.1.1");               // true

// 纯数字
Validator.isNumber("12345");                    // true

// 车牌号
Validator.isPlateNumber("京A12345");            // true

// 邮编
Validator.isZipCode("100000");                  // true
```

**常用 PatternPool 常量**：

| 常量 | 正则 | 用途 |
|------|------|------|
| `PatternPool.EMAIL` | 邮箱正则 | 邮箱验证 |
| `PatternPool.MOBILE` | 手机号正则 | 手机号验证 |
| `PatternPool.CHINESE` | 中文正则 | 中文检测 |
| `PatternPool.URL` | URL 正则 | URL 验证 |
| `PatternPool.IPV4` | IPv4 正则 | IP 验证 |
| `PatternPool.NUMBERS` | 纯数字 | 数字检测 |
| `PatternPool.BIRTHDAY` | 生日格式 | 生日验证 |
| `PatternPool.CITIZEN_ID` | 身份证 | 身份证验证 |
| `PatternPool.PLATE_NUMBER` | 车牌号 | 车牌验证 |

## 常见问题 FAQ

### Q: isMatch 和 contains 有什么区别？
**A**: `isMatch` 要求**整个字符串完全匹配**正则；`contains` 只要求字符串**包含**匹配的部分。

### Q: PatternPool 的 Pattern 会缓存吗？
**A**: 是的，`PatternPool` 内部做了缓存（`WeakHashMap`），相同正则不会重复编译。

### Q: 如何使用命名分组？
**A**: 使用 `(?<name>pattern)` 语法定义命名分组，通过 `ReUtil.get(regex, content, "name")` 获取。

### Q: 正则性能注意事项？
**A**: 避免在循环中反复编译正则（`Pattern.compile`），使用 `PatternPool.get(regex)` 获取缓存的 Pattern。避免灾难性回溯（如 `(a+)+b`）。

## 最佳实践

1. **数据校验用 `Validator`**：邮箱、手机号等不用自己写正则。
2. **用户输入构建正则前先 `escape`**：防止正则注入。
3. **提取数据用 `findAll` + 分组**：比手写 Matcher 循环简洁。
4. **复用 Pattern 用 `PatternPool.get()`**：避免重复编译。
5. **简单包含判断用 `contains`**：比 `isMatch` 更直观。
