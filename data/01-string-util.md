---
title: 字符串工具
classes:
  - cn.hutool.core.util.StrUtil
  - cn.hutool.core.text.CharSequenceUtil
since: 4.0.0
tags: [字符串, string, text, StrUtil, CharSequenceUtil]
---

# 字符串工具 — StrUtil / CharSequenceUtil

## 概述

`StrUtil` 是 Hutool 中使用频率最高的工具类，提供了丰富的字符串操作方法。它继承自 `CharSequenceUtil`，所有方法均为静态方法，对 `null` 有良好的容错处理（大部分方法传入 `null` 不会抛出 NPE）。

`CharSequenceUtil` 是底层实现类，接受 `CharSequence` 类型参数；`StrUtil` 在其基础上封装，部分方法直接返回 `String` 类型，日常开发推荐直接使用 `StrUtil`。

## Maven 依赖

```xml
<!-- Hutool 5.x -->
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>

<!-- 或引入 hutool-all -->
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `isBlank(str)` | 判断是否为空白（null、空串、纯空白字符） | `boolean` | 3.0+ |
| `isNotBlank(str)` | 判断是否不为空白 | `boolean` | 3.0+ |
| `isEmpty(str)` | 判断是否为空（null 或空串） | `boolean` | 3.0+ |
| `isNotEmpty(str)` | 判断是否不为空 | `boolean` | 3.0+ |
| `hasBlank(strs...)` | 是否有任一为空白 | `boolean` | 4.0+ |
| `hasEmpty(strs...)` | 是否有任一为空 | `boolean` | 4.0+ |
| `isAllBlank(strs...)` | 是否全部为空白 | `boolean` | 5.3+ |
| `isAllNotBlank(strs...)` | 是否全部不为空白 | `boolean` | 5.3+ |
| `trim(str)` | 去除两端空白 | `String` | 3.0+ |
| `trimStart(str)` | 去除左端空白 | `String` | 5.4+ |
| `trimEnd(str)` | 去除右端空白 | `String` | 5.4+ |
| `nullToEmpty(str)` | null 转空串 | `String` | 3.0+ |
| `emptyToNull(str)` | 空串转 null | `String` | 3.0+ |
| `nullToDefault(str, default)` | null 转默认值 | `String` | 4.0+ |
| `format(template, params...)` | {} 占位符格式化 | `String` | 3.0+ |
| `split(str, separator)` | 拆分字符串 | `String[]` | 3.0+ |
| `join(separator, parts...)` | 连接字符串 | `String` | 3.0+ |
| `sub(str, from, to)` | 安全截取子串（支持负数索引） | `String` | 3.0+ |
| `contains(str, searchStr)` | 是否包含子串 | `boolean` | 3.0+ |
| `containsIgnoreCase(str, searchStr)` | 忽略大小写包含判断 | `boolean` | 4.0+ |
| `containsAny(str, searchStrs...)` | 包含任一子串 | `boolean` | 4.0+ |
| `startWith(str, prefix)` | 是否以指定前缀开头 | `boolean` | 3.0+ |
| `endWith(str, suffix)` | 是否以指定后缀结尾 | `boolean` | 3.0+ |
| `removePrefix(str, prefix)` | 移除前缀 | `String` | 3.0+ |
| `removeSuffix(str, suffix)` | 移除后缀 | `String` | 3.0+ |
| `removePrefixIgnoreCase(str, prefix)` | 忽略大小写移除前缀 | `String` | 4.0+ |
| `toUnderlineCase(str)` | 驼峰转下划线 | `String` | 3.0+ |
| `toCamelCase(str)` | 下划线转驼峰 | `String` | 3.0+ |
| `repeat(str, count)` | 重复字符串 | `String` | 3.0+ |
| `padPre(str, length, padChar)` | 左补齐 | `String` | 4.0+ |
| `padAfter(str, length, padChar)` | 右补齐 | `String` | 4.0+ |
| `brief(str, maxLength)` | 缩略显示 | `String` | 5.0+ |
| `replace(str, from, to)` | 替换字符串 | `String` | 3.0+ |
| `upperFirst(str)` | 首字母大写 | `String` | 3.0+ |
| `lowerFirst(str)` | 首字母小写 | `String` | 3.0+ |
| `addPrefixIfNot(str, prefix)` | 安全添加前缀 | `String` | 5.4+ |
| `addSuffixIfNot(str, suffix)` | 安全添加后缀 | `String` | 5.4+ |
| `count(str, searchStr)` | 统计子串出现次数 | `int` | 5.0+ |
| `reverse(str)` | 反转字符串 | `String` | 4.0+ |
| `maxLength(str, maxLength)` | 限制最大长度并截取 | `String` | 5.5+ |
| `hide(str, start, end)` | 隐藏中间字符（用 * 替代） | `String` | 5.6+ |

## 详细 API 与示例

### isBlank / isEmpty — 判空系列

```java
import cn.hutool.core.util.StrUtil;

// isBlank: null、""、"  " 都返回 true
StrUtil.isBlank(null);    // true
StrUtil.isBlank("");      // true
StrUtil.isBlank("  ");    // true
StrUtil.isBlank("abc");   // false

// isEmpty: 仅 null、"" 返回 true，"  " 返回 false
StrUtil.isEmpty(null);    // true
StrUtil.isEmpty("");      // true
StrUtil.isEmpty("  ");    // false  ← 与 isBlank 的区别！
StrUtil.isEmpty("abc");   // false

// 批量判断
StrUtil.hasBlank("abc", "", "def");      // true，有一个为空白
StrUtil.isAllNotBlank("abc", "def");     // true，全部不为空白
```

> ⚠️ **isBlank 与 isEmpty 的区别**：`isBlank` 认为纯空白字符（空格、制表符等）也是"空"，`isEmpty` 则不认为。日常业务中推荐使用 `isBlank`。

### format — {} 占位符格式化

```java
import cn.hutool.core.util.StrUtil;

// 类似 SLF4J 的 {} 占位符风格
String result = StrUtil.format("Hello, {}! Today is {}.", "Hutool", "Monday");
// => "Hello, Hutool! Today is Monday."

// 可用于拼接日志、异常消息等场景，比 String.format 更简洁
String msg = StrUtil.format("用户 {} 在 {} 执行了 {} 操作", userId, time, action);
```

> ⚠️ **注意**：`format` 不支持 `%s`、`%d` 等 printf 风格占位符，仅支持 `{}`。如需 printf 风格，使用 `String.format()`。

### split — 拆分字符串

```java
import cn.hutool.core.util.StrUtil;

// 基础拆分
String[] parts = StrUtil.split("a,b,c,d", ",");
// => ["a", "b", "c", "d"]

// 限制拆分数量
String[] parts2 = StrUtil.split("a,b,c,d", ",", 2);
// => ["a", "b,c,d"]

// 自动去除空白项
List<String> list = StrUtil.splitTrim("a , b , c", ",");
// => ["a", "b", "c"]

// null 安全
String[] nullResult = StrUtil.split(null, ",");
// => null（不会抛 NPE）
```

> 💡 **对比 JDK**：`String.split()` 传入 null 会抛 NPE，且不支持自动 trim；`StrUtil.split()` 对 null 友好，且提供 `splitTrim` 变体。

### sub — 安全截取子串

```java
import cn.hutool.core.util.StrUtil;

String str = "Hello, Hutool!";

// 正常截取（from 包含，to 不包含）
StrUtil.sub(str, 0, 5);    // => "Hello"

// 支持负数索引（从末尾倒数）
StrUtil.sub(str, -7, -1);  // => "Hutool"

// 越界自动修正，不会抛 IndexOutOfBoundsException
StrUtil.sub(str, 0, 100);  // => "Hello, Hutool!"

// null 安全
StrUtil.sub(null, 0, 5);   // => null
```

### removePrefix / removeSuffix — 移除前后缀

```java
import cn.hutool.core.util.StrUtil;

StrUtil.removePrefix("Hello World", "Hello ");    // => "World"
StrUtil.removeSuffix("file.txt", ".txt");          // => "file"

// 不匹配时返回原字符串
StrUtil.removePrefix("Hello World", "xxx");        // => "Hello World"

// 忽略大小写
StrUtil.removePrefixIgnoreCase("Hello World", "hello ");  // => "World"
```

### toUnderlineCase / toCamelCase — 命名风格互转

```java
import cn.hutool.core.util.StrUtil;

// 驼峰 → 下划线
StrUtil.toUnderlineCase("userName");       // => "user_name"
StrUtil.toUnderlineCase("userNameAndAge"); // => "user_name_and_age"
StrUtil.toUnderlineCase("HTMLParser");     // => "html_parser"

// 下划线 → 驼峰
StrUtil.toCamelCase("user_name");          // => "userName"
StrUtil.toCamelCase("user_name_and_age");  // => "userNameAndAge"
```

### padPre / padAfter — 字符串补齐

```java
import cn.hutool.core.util.StrUtil;

// 左补零
StrUtil.padPre("123", 6, '0');    // => "000123"

// 右补空格
StrUtil.padAfter("abc", 8, ' ');  // => "abc     "

// 已经足够长则不截断
StrUtil.padPre("123456", 4, '0'); // => "123456"（不截断）
```

### brief — 缩略显示

```java
import cn.hutool.core.util.StrUtil;

// 超长字符串缩略为指定长度，中间用 "..." 替代
StrUtil.brief("This is a very long text that needs to be shortened.", 20);
// => "This is...shortened."
```

### addPrefixIfNot / addSuffixIfNot — 安全添加前后缀

```java
import cn.hutool.core.util.StrUtil;

// 只在没有该前缀时才添加
StrUtil.addPrefixIfNot("/api/user", "/");      // => "/api/user"（已有，不重复添加）
StrUtil.addPrefixIfNot("api/user", "/");       // => "/api/user"（添加）

// 同理，后缀
StrUtil.addSuffixIfNot("image.png", ".png");   // => "image.png"
StrUtil.addSuffixIfNot("image", ".png");       // => "image.png"
```

### upperFirst / lowerFirst — 首字母大小写

```java
import cn.hutool.core.util.StrUtil;

StrUtil.upperFirst("hello");  // => "Hello"
StrUtil.lowerFirst("Hello");  // => "hello"
StrUtil.upperFirst(null);     // => null（null 安全）
```

### hide — 字符隐藏

```java
import cn.hutool.core.util.StrUtil;

// 隐藏手机号中间4位
StrUtil.hide("13812345678", 3, 7);  // => "138****5678"

// 隐藏邮箱
StrUtil.hide("test@example.com", 1, 4);  // => "t***@example.com"
```

### nullToEmpty / emptyToNull / nullToDefault — 空值转换

```java
import cn.hutool.core.util.StrUtil;

StrUtil.nullToEmpty(null);            // => ""
StrUtil.emptyToNull("");              // => null
StrUtil.nullToDefault(null, "默认值"); // => "默认值"
StrUtil.nullToDefault("abc", "默认值"); // => "abc"
```

### repeat — 重复字符串

```java
import cn.hutool.core.util.StrUtil;

StrUtil.repeat("ab", 3);    // => "ababab"
StrUtil.repeat('*', 5);     // => "*****"
```

### replace — 字符串替换

```java
import cn.hutool.core.util.StrUtil;

StrUtil.replace("Hello World", "World", "Hutool");  // => "Hello Hutool"

// null 安全
StrUtil.replace(null, "a", "b");  // => null
```

### count — 统计出现次数

```java
import cn.hutool.core.util.StrUtil;

StrUtil.count("abcabc", "abc");  // => 2
StrUtil.count("aaa", "aa");     // => 1（不重叠计数）
```

## 常见问题 FAQ

### Q: isBlank 和 isEmpty 到底该用哪个？
**A**: 日常业务开发推荐 `isBlank`。它把 `"  "`（纯空格）也当作空值，更贴合实际业务需求。`isEmpty` 仅判断 null 和空串，适合明确需要区分空格的场景。

### Q: format 支持数字格式化吗（如保留两位小数）？
**A**: `StrUtil.format` 仅支持 `{}` 简单占位，不支持格式化。需要数字格式化请使用 `NumberUtil.decimalFormat()` 或 `String.format()`。

### Q: split 和 JDK 的 String.split 有什么区别？
**A**:
- `StrUtil.split(null, ",")` 返回 null，JDK 版本会抛 NPE
- `StrUtil.splitTrim()` 可自动 trim 每个元素
- Hutool 版本不使用正则（性能更高），JDK 版本使用正则

### Q: sub 支持 UTF-8 中文吗？
**A**: 支持。`sub` 基于字符索引（不是字节），中文、emoji 等多字节字符均正常处理。

### Q: toCamelCase 对连续大写（如 HTML、IO）的处理？
**A**: `toUnderlineCase("HTMLParser")` → `"html_parser"`。连续大写字母会被视为一个整体词。

## 最佳实践

1. **判空首选 `isBlank`**：业务代码中 99% 的场景使用 `isBlank` / `isNotBlank` 即可。
2. **日志拼接用 `format`**：避免 `"用户" + userId + "执行了" + action` 这种低效拼接，使用 `StrUtil.format("用户 {} 执行了 {}", userId, action)`。
3. **路径拼接用 `addSuffixIfNot`**：避免路径出现 `//` 双斜杠。
4. **数据库字段映射用命名转换**：DO 对象驼峰命名与数据库下划线命名互转时，使用 `toUnderlineCase` / `toCamelCase`。
5. **敏感信息脱敏用 `hide`**：日志中打印手机号、身份证等敏感信息时，使用 `hide` 进行部分隐藏。
