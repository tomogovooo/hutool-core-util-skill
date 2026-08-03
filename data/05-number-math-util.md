---
title: 数字与数学工具
classes:
  - cn.hutool.core.util.NumberUtil
  - cn.hutool.core.math.MathUtil
since: 3.0.0
tags: [数字, 数学, number, math, BigDecimal, 计算, 精度]
---

# 数字与数学工具 — NumberUtil / MathUtil

## 概述

- **`NumberUtil`**：数字工具类，核心价值是基于 `BigDecimal` 的精确四则运算（解决浮点数精度问题），以及数字格式化、类型判断、安全解析等。
- **`MathUtil`**：数学工具类，提供排列组合计算等数学函数。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### NumberUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `add(v1, v2)` | 精确加法 | `BigDecimal` | 3.0+ |
| `sub(v1, v2)` | 精确减法 | `BigDecimal` | 3.0+ |
| `mul(v1, v2)` | 精确乘法 | `BigDecimal` | 3.0+ |
| `div(v1, v2)` | 精确除法（默认10位精度） | `BigDecimal` | 3.0+ |
| `div(v1, v2, scale)` | 指定精度除法 | `BigDecimal` | 3.0+ |
| `div(v1, v2, scale, roundingMode)` | 指定精度和舍入模式 | `BigDecimal` | 5.4+ |
| `round(v, scale)` | 四舍五入 | `BigDecimal` | 3.0+ |
| `roundStr(v, scale)` | 四舍五入并转字符串 | `String` | 3.0+ |
| `roundHalfEven(v, scale)` | 银行家舍入 | `BigDecimal` | 5.0+ |
| `decimalFormat(pattern, v)` | 数字格式化 | `String` | 3.0+ |
| `isNumber(str)` | 是否数字（含小数、负数） | `boolean` | 3.0+ |
| `isInteger(str)` | 是否整数 | `boolean` | 3.0+ |
| `isDouble(str)` | 是否小数 | `boolean` | 5.4+ |
| `isLong(str)` | 是否 Long 范围 | `boolean` | 3.0+ |
| `isPrimes(n)` | 是否素数 | `boolean` | 4.0+ |
| `parseInt(str)` | 安全解析 int | `int` | 3.0+ |
| `parseLong(str)` | 安全解析 long | `long` | 3.0+ |
| `parseDouble(str)` | 安全解析 double | `double` | 5.4+ |
| `parseNumber(str)` | 安全解析 Number | `Number` | 5.5+ |
| `toStr(number)` | 数字转字符串（去尾零） | `String` | 5.4+ |
| `toBigDecimal(v)` | 转 BigDecimal | `BigDecimal` | 5.4+ |
| `max(values...)` | 最大值 | `BigDecimal` | 3.0+ |
| `min(values...)` | 最小值 | `BigDecimal` | 3.0+ |
| `factorial(n)` | 阶乘 | `BigInteger` | 4.0+ |
| `divisor(a, b)` | 最大公约数 | `int` | 4.0+ |
| `multiple(a, b)` | 最小公倍数 | `int` | 4.0+ |
| `range(start, stop, step)` | 数字范围 | `int[]` | 5.4+ |
| `compare(a, b)` | null 安全比较 | `int` | 5.4+ |
| `equals(a, b)` | 值相等比较（忽略精度） | `boolean` | 5.6+ |
| `generateRandomNumber(start, end, size)` | 不重复随机数 | `int[]` | 4.0+ |

### MathUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `arrangementCount(n, m)` | 排列数 A(n,m) | `long` | 5.4+ |
| `combinationCount(n, m)` | 组合数 C(n,m) | `long` | 5.4+ |
| `arrangementSelect(data, m)` | 排列选择 | `List<String[]>` | 5.4+ |
| `combinationSelect(data, m)` | 组合选择 | `List<String[]>` | 5.4+ |

## 详细 API 与示例

### 精确四则运算 — 解决浮点数精度问题

```java
import cn.hutool.core.util.NumberUtil;

// 经典浮点数精度问题
System.out.println(0.1 + 0.2);           // 0.30000000000000004 ❌

// Hutool 精确计算
BigDecimal result = NumberUtil.add(0.1, 0.2);  // 0.3 ✅
BigDecimal sub = NumberUtil.sub(1.0, 0.9);     // 0.1 ✅
BigDecimal mul = NumberUtil.mul(0.1, 3);       // 0.3 ✅

// 除法（必须指定精度，否则除不尽会抛异常）
BigDecimal div = NumberUtil.div(10, 3, 2);     // 3.33
BigDecimal div2 = NumberUtil.div(10, 3, 4, RoundingMode.HALF_UP); // 3.3333

// 支持多参数
BigDecimal sum = NumberUtil.add(0.1, 0.2, 0.3);  // 0.6
```

> ⚠️ **注意**：`div` 不指定 `scale` 时默认保留 10 位。如果除法结果是无限小数且未指定精度，会抛 `ArithmeticException`。**强烈建议始终指定 scale**。

### round / roundHalfEven — 舍入

```java
import cn.hutool.core.util.NumberUtil;

// 四舍五入，保留2位小数
NumberUtil.round(3.1415, 2);       // 3.14
NumberUtil.round(3.145, 2);        // 3.15
NumberUtil.roundStr(3.1415, 2);    // "3.14"

// 银行家舍入（四舍六入五成双），财务常用
NumberUtil.roundHalfEven(2.225, 2);  // 2.22（5前为偶数，舍）
NumberUtil.roundHalfEven(2.235, 2);  // 2.24（5前为奇数，入）
```

> 💡 **银行家舍入**：`HALF_EVEN` 模式在统计学和金融领域更准确，避免了传统四舍五入的系统性偏差。

### decimalFormat — 数字格式化

```java
import cn.hutool.core.util.NumberUtil;

// 千分位格式
NumberUtil.decimalFormat(",###", 1234567);       // "1,234,567"

// 保留2位小数
NumberUtil.decimalFormat("#.##", 3.1);            // "3.1"
NumberUtil.decimalFormat("0.00", 3.1);            // "3.10"（补零）

// 百分比
NumberUtil.decimalFormat("#.##%", 0.8567);        // "85.67%"

// 金额格式
NumberUtil.decimalFormat(",##0.00", 1234567.5);   // "1,234,567.50"
```

### 类型判断与安全解析

```java
import cn.hutool.core.util.NumberUtil;

// 类型判断
NumberUtil.isNumber("123");      // true
NumberUtil.isNumber("12.3");     // true
NumberUtil.isNumber("-12.3");    // true
NumberUtil.isNumber("abc");      // false
NumberUtil.isNumber("");         // false
NumberUtil.isNumber(null);       // false

NumberUtil.isInteger("123");     // true
NumberUtil.isInteger("12.3");    // false

// 安全解析（失败返回默认值，不抛异常）
int n = NumberUtil.parseInt("123");     // 123
int n2 = NumberUtil.parseInt("abc");    // 0（解析失败返回0）
long l = NumberUtil.parseLong("999");   // 999
```

### toStr — 数字转字符串

```java
import cn.hutool.core.util.NumberUtil;

// 去除尾部多余的零
NumberUtil.toStr(new BigDecimal("3.1400"));  // "3.14"
NumberUtil.toStr(new BigDecimal("100.00"));  // "100"
NumberUtil.toStr(new BigDecimal("0.50"));    // "0.5"
```

### 数学工具

```java
import cn.hutool.core.util.NumberUtil;
import cn.hutool.core.math.MathUtil;

// 阶乘
NumberUtil.factorial(5);    // 120

// 最大公约数
NumberUtil.divisor(12, 8);  // 4

// 最小公倍数
NumberUtil.multiple(4, 6);  // 12

// 排列数 A(5,2) = 20
MathUtil.arrangementCount(5, 2);

// 组合数 C(5,2) = 10
MathUtil.combinationCount(5, 2);

// 数字范围
int[] range = NumberUtil.range(1, 10, 2);  // [1, 3, 5, 7, 9]
```

### compare / equals — 数字比较

```java
import cn.hutool.core.util.NumberUtil;

// null 安全比较
NumberUtil.compare(1, 2);     // -1
NumberUtil.compare(null, 1);  // 不会 NPE

// BigDecimal 值比较（忽略精度）
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("1.00");
a.equals(b);                  // false ❌（精度不同）
NumberUtil.equals(a, b);      // true  ✅（值相同）
```

## 常见问题 FAQ

### Q: 为什么不直接用 double 运算？
**A**: IEEE 754 浮点数无法精确表示部分十进制小数（如 0.1），导致 `0.1 + 0.2 != 0.3`。`NumberUtil` 内部用 `BigDecimal` 确保精度。

### Q: div 不指定 scale 会怎样？
**A**: 如果结果是无限小数（如 10/3），会抛 `ArithmeticException`。**始终指定 scale**。

### Q: BigDecimal 的 equals 为什么不能用？
**A**: `BigDecimal` 的 `equals` 会同时比较值和精度，`1.0` 和 `1.00` 不相等。用 `NumberUtil.equals()` 或 `BigDecimal.compareTo() == 0`。

### Q: roundHalfEven 什么场景用？
**A**: 财务系统、银行利率计算等需要统计学无偏舍入的场景。

## 最佳实践

1. **金额计算必用 `BigDecimal`**：通过 `NumberUtil.add/sub/mul/div` 操作。
2. **除法必指定精度**：`NumberUtil.div(a, b, 2, RoundingMode.HALF_UP)`。
3. **金融场景用银行家舍入**：`roundHalfEven` 而非 `round`。
4. **数字比较用 `NumberUtil.equals`**：避免 `BigDecimal.equals` 的精度陷阱。
5. **前端展示用 `decimalFormat`**：统一格式化数字为千分位或百分比。
6. **字符串转数字用安全方法**：`parseInt` / `parseLong` 而非 `Integer.parseInt`。
