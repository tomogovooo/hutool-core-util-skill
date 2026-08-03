---
title: 类型转换工具
classes:
  - cn.hutool.core.convert.Convert
since: 3.0.0
tags: [转换, convert, 类型转换, type, Convert]
---

# 类型转换工具 — Convert

## 概述

`Convert` 是 Hutool 的万能类型转换工具类。它能将任意类型的值转换为目标类型，支持基本类型、集合、日期、枚举、十六进制、Unicode、全角半角、中文数字等多种转换。核心特性是**转换失败不抛异常**（quiet 模式），返回默认值。

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
| `toStr(value)` | 转字符串 | `String` | 3.0+ |
| `toStr(value, defaultValue)` | 转字符串（带默认值） | `String` | 3.0+ |
| `toInt(value)` | 转 Integer | `Integer` | 3.0+ |
| `toInt(value, defaultValue)` | 转 Integer（带默认值） | `Integer` | 3.0+ |
| `toIntArray(value)` | 转 Integer 数组 | `Integer[]` | 4.0+ |
| `toLong(value)` | 转 Long | `Long` | 3.0+ |
| `toLong(value, defaultValue)` | 转 Long（带默认值） | `Long` | 3.0+ |
| `toLongArray(value)` | 转 Long 数组 | `Long[]` | 4.0+ |
| `toDouble(value)` | 转 Double | `Double` | 3.0+ |
| `toDoubleArray(value)` | 转 Double 数组 | `Double[]` | 4.0+ |
| `toFloat(value)` | 转 Float | `Float` | 3.0+ |
| `toBool(value)` | 转 Boolean | `Boolean` | 3.0+ |
| `toBigDecimal(value)` | 转 BigDecimal | `BigDecimal` | 5.4+ |
| `toBigInteger(value)` | 转 BigInteger | `BigInteger` | 5.4+ |
| `toChar(value)` | 转 Character | `Character` | 3.0+ |
| `toNumber(value)` | 转 Number | `Number` | 4.0+ |
| `toList(value)` | 转 List | `List` | 3.0+ |
| `toSet(value)` | 转 Set | `Set` | 5.4+ |
| `toMap(value)` | 转 Map | `Map` | 5.4+ |
| `toDate(value)` | 转 Date | `Date` | 5.0+ |
| `toLocalDateTime(value)` | 转 LocalDateTime | `LocalDateTime` | 5.4+ |
| `toEnum(clazz, value)` | 转枚举 | `Enum` | 5.0+ |
| `convert(type, value)` | 泛型转换 | `T` | 3.0+ |
| `convert(type, value, defaultValue)` | 泛型转换（带默认值） | `T` | 3.0+ |
| `convertQuietly(type, value, defaultValue)` | 静默转换（不抛异常） | `T` | 5.4+ |
| `toHex(value)` | 字符串转十六进制 | `String` | 4.0+ |
| `hexToStr(hexStr)` | 十六进制转字符串 | `String` | 4.0+ |
| `toUnicode(str)` | 字符串转 Unicode | `String` | 4.0+ |
| `unicodeToStr(unicode)` | Unicode 转字符串 | `String` | 4.0+ |
| `toSBC(str)` | 半角转全角 | `String` | 4.0+ |
| `toDBC(str)` | 全角转半角 | `String` | 4.0+ |
| `numberToChinese(number, isUseTraditional)` | 数字转中文 | `String` | 5.0+ |
| `chineseToNumber(chineseNumber)` | 中文转数字 | `int` | 5.0+ |
| `digitToChinese(amount)` | 金额转大写 | `String` | 3.0+ |
| `convertCharset(str, from, to)` | 字符集转换 | `String` | 3.0+ |
| `convertTime(value, fromUnit, toUnit)` | 时间单位转换 | `long` | 5.4+ |

## 详细 API 与示例

### 基本类型转换

```java
import cn.hutool.core.convert.Convert;

// 字符串转各种类型
int intVal = Convert.toInt("123");           // 123
long longVal = Convert.toLong("99999");       // 99999
double doubleVal = Convert.toDouble("3.14");  // 3.14
float floatVal = Convert.toFloat("1.5");      // 1.5
boolean boolVal = Convert.toBool("true");     // true
char charVal = Convert.toChar("A");           // 'A'

// 带默认值（转换失败返回默认值，不抛异常）
int safe = Convert.toInt("abc", 0);           // 0（解析失败）
long safe2 = Convert.toLong(null, -1L);       // -1

// Boolean 的智能转换
Convert.toBool("yes");   // true
Convert.toBool("1");     // true
Convert.toBool("on");    // true
Convert.toBool("no");    // false
Convert.toBool("0");     // false
Convert.toBool("off");   // false
```

### 数组转换

```java
import cn.hutool.core.convert.Convert;

// 字符串数组转整型数组
String[] strArr = {"1", "2", "3", "4"};
Integer[] intArr = Convert.toIntArray(strArr);    // [1, 2, 3, 4]
Long[] longArr = Convert.toLongArray(strArr);     // [1L, 2L, 3L, 4L]
Double[] dblArr = Convert.toDoubleArray(strArr);  // [1.0, 2.0, 3.0, 4.0]

// 逗号分隔字符串转数组
Integer[] fromStr = Convert.toIntArray("1,2,3,4");
```

### 集合转换

```java
import cn.hutool.core.convert.Convert;

// 数组转 List
Object[] arr = {"a", "b", "c"};
List<?> list = Convert.toList(arr);

// 字符串转 List
List<?> list2 = Convert.toList("a,b,c");
```

### 日期转换

```java
import cn.hutool.core.convert.Convert;

// 字符串转 Date
Date date = Convert.toDate("2024-01-15");
Date dateTime = Convert.toDate("2024-01-15 14:30:25");

// 时间戳转 Date
Date fromTs = Convert.toDate(1705296625000L);

// 转 LocalDateTime
LocalDateTime ldt = Convert.toLocalDateTime("2024-01-15 14:30:25");
```

### 枚举转换

```java
import cn.hutool.core.convert.Convert;

// 字符串或序号转枚举
enum Season { SPRING, SUMMER, AUTUMN, WINTER }

Season s1 = Convert.toEnum(Season.class, "SPRING");  // Season.SPRING
Season s2 = Convert.toEnum(Season.class, 1);          // Season.SUMMER（按序号）
```

### 泛型转换 — convert / convertQuietly

```java
import cn.hutool.core.convert.Convert;
import cn.hutool.core.lang.TypeReference;

// 泛型转换
Integer num = Convert.convert(Integer.class, "123");
BigDecimal bd = Convert.convert(BigDecimal.class, "3.14");

// 转换为带泛型的类型（使用 TypeReference）
List<String> list = Convert.convert(
    new TypeReference<List<String>>() {}, "a,b,c");

// 静默转换（失败返回默认值，绝不抛异常）
Integer safe = Convert.convertQuietly(Integer.class, "不是数字", 0);
// => 0
```

> ⚠️ **convert vs convertQuietly**：`convert` 在无法转换时**可能抛出异常**，`convertQuietly` 在失败时**静默返回默认值**。不确定数据来源时推荐用 `convertQuietly`。

### 十六进制转换

```java
import cn.hutool.core.convert.Convert;

// 字符串 → 十六进制
String hex = Convert.toHex("Hello", CharsetUtil.CHARSET_UTF_8);
// => "48656c6c6f"

// 十六进制 → 字符串
String str = Convert.hexToStr("48656c6c6f", CharsetUtil.CHARSET_UTF_8);
// => "Hello"
```

### Unicode 转换

```java
import cn.hutool.core.convert.Convert;

// 字符串 → Unicode
String unicode = Convert.toUnicode("你好");
// => "\\u4f60\\u597d"

// Unicode → 字符串
String str = Convert.unicodeToStr("\\u4f60\\u597d");
// => "你好"
```

### 全角半角转换

```java
import cn.hutool.core.convert.Convert;

// 半角 → 全角
String sbc = Convert.toSBC("Hello123");
// => "Ｈｅｌｌｏ１２３"

// 全角 → 半角
String dbc = Convert.toDBC("Ｈｅｌｌｏ１２３");
// => "Hello123"
```

### 中文数字转换

```java
import cn.hutool.core.convert.Convert;

// 数字 → 中文
Convert.numberToChinese(12345, false);
// => "一万二千三百四十五"

Convert.numberToChinese(12345, true);
// => "壹万贰仟叁佰肆拾伍"（传统大写）

// 中文 → 数字
Convert.chineseToNumber("一万二千三百四十五");
// => 12345

// 金额大写（财务常用）
Convert.digitToChinese(123456.78);
// => "壹拾贰万叁仟肆佰伍拾陆元柒角捌分"
```

### 字符集转换

```java
import cn.hutool.core.convert.Convert;

// 编码转换
String result = Convert.convertCharset(str, "GBK", "UTF-8");
```

### 时间单位转换

```java
import cn.hutool.core.convert.Convert;
import java.util.concurrent.TimeUnit;

// 毫秒转秒
long seconds = Convert.convertTime(5000, TimeUnit.MILLISECONDS, TimeUnit.SECONDS);
// => 5

// 小时转分钟
long minutes = Convert.convertTime(2, TimeUnit.HOURS, TimeUnit.MINUTES);
// => 120
```

## 常见问题 FAQ

### Q: convert 和 toXxx 方法有什么区别？
**A**: `toXxx` 是 `convert` 的便捷封装，功能一样。`convert` 更灵活，支持任意目标类型。

### Q: 如何自定义转换规则？
**A**: 通过 `ConverterRegistry.getInstance().putCustom(targetType, converter)` 注册自定义转换器。

### Q: toBool 能识别哪些值？
**A**: `true`/`yes`/`on`/`1`/`ok` 为 true；`false`/`no`/`off`/`0` 为 false。不区分大小写。

### Q: convertQuietly 什么场景用？
**A**: 外部输入（用户输入、HTTP 参数、配置文件解析）等数据来源不可信的场景。

## 最佳实践

1. **HTTP 参数转换用 `Convert`**：`Convert.toInt(request.getParameter("page"), 1)`。
2. **不可信数据用 `convertQuietly`**：避免异常中断业务流程。
3. **金额大写用 `digitToChinese`**：财务系统、发票、支票等场景直接调用。
4. **编码问题用 `convertCharset`**：处理老系统 GBK 数据时特别有用。
5. **泛型集合用 `TypeReference`**：`Convert.convert(new TypeReference<List<Integer>>(){}, data)`。
