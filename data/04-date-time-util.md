---
title: 日期时间工具
classes:
  - cn.hutool.core.date.DateUtil
  - cn.hutool.core.date.LocalDateTimeUtil
since: 3.0.0
tags: [日期, 时间, date, time, datetime, DateUtil, LocalDateTimeUtil]
---

# 日期时间工具 — DateUtil / LocalDateTimeUtil

## 概述

- **`DateUtil`**：基于 `java.util.Date` 的日期工具类，提供日期创建、解析、格式化、偏移、比较等全套操作。内部使用线程安全的 `FastDateFormat` 替代 `SimpleDateFormat`。
- **`LocalDateTimeUtil`**：基于 Java 8 `java.time.LocalDateTime` 的工具类，适合 Java 8+ 项目。

两者功能类似，新项目推荐使用 `LocalDateTimeUtil`。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### DateUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `date()` | 当前日期 DateTime | `DateTime` | 3.0+ |
| `now()` | 当前时间字符串 yyyy-MM-dd HH:mm:ss | `String` | 3.0+ |
| `today()` | 当前日期字符串 yyyy-MM-dd | `String` | 4.0+ |
| `parse(dateStr)` | 智能解析日期字符串 | `DateTime` | 3.0+ |
| `parse(dateStr, format)` | 指定格式解析 | `DateTime` | 3.0+ |
| `format(date, format)` | 格式化日期 | `String` | 3.0+ |
| `formatDateTime(date)` | 格式化为 yyyy-MM-dd HH:mm:ss | `String` | 3.0+ |
| `formatDate(date)` | 格式化为 yyyy-MM-dd | `String` | 3.0+ |
| `formatTime(date)` | 格式化为 HH:mm:ss | `String` | 3.0+ |
| `year(date)` | 获取年份 | `int` | 3.0+ |
| `month(date)` | 获取月份（从0开始） | `int` | 3.0+ |
| `dayOfMonth(date)` | 获取日 | `int` | 3.0+ |
| `hour(date, is24h)` | 获取小时 | `int` | 3.0+ |
| `minute(date)` | 获取分钟 | `int` | 3.0+ |
| `second(date)` | 获取秒 | `int` | 3.0+ |
| `dayOfWeek(date)` | 获取星期几 | `int` | 3.0+ |
| `beginOfDay(date)` | 当天开始 00:00:00 | `DateTime` | 3.0+ |
| `endOfDay(date)` | 当天结束 23:59:59 | `DateTime` | 3.0+ |
| `beginOfMonth(date)` | 月初 | `DateTime` | 3.0+ |
| `endOfMonth(date)` | 月末 | `DateTime` | 3.0+ |
| `beginOfYear(date)` | 年初 | `DateTime` | 3.0+ |
| `endOfYear(date)` | 年末 | `DateTime` | 3.0+ |
| `beginOfWeek(date)` | 周一 | `DateTime` | 4.0+ |
| `endOfWeek(date)` | 周日 | `DateTime` | 4.0+ |
| `offset(date, dateField, offset)` | 日期偏移 | `DateTime` | 3.0+ |
| `offsetDay(date, offset)` | 偏移天 | `DateTime` | 3.0+ |
| `offsetMonth(date, offset)` | 偏移月 | `DateTime` | 3.0+ |
| `offsetHour(date, offset)` | 偏移小时 | `DateTime` | 3.0+ |
| `between(begin, end, unit)` | 两日期间差值 | `long` | 3.0+ |
| `betweenDay(begin, end, isReset)` | 天数差 | `long` | 5.4+ |
| `isLeapYear(year)` | 是否闰年 | `boolean` | 3.0+ |
| `age(birthday, dateToCompare)` | 计算年龄 | `int` | 3.0+ |
| `isIn(date, begin, end)` | 日期是否在范围内 | `boolean` | 5.4+ |
| `isSameDay(date1, date2)` | 是否同一天 | `boolean` | 4.0+ |
| `current()` | 当前时间戳（毫秒） | `long` | 5.0+ |
| `currentSeconds()` | 当前时间戳（秒） | `long` | 5.0+ |
| `zodiac(month, day)` | 星座 | `String` | 4.0+ |
| `getChineseZodiac(year)` | 生肖 | `String` | 4.0+ |
| `compare(date1, date2)` | 日期比较 | `int` | 5.4+ |
| `timer()` | 创建计时器 | `TimeInterval` | 4.0+ |

### LocalDateTimeUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `now()` | 当前时间 | `LocalDateTime` | 5.4+ |
| `of(date)` | Date 转 LocalDateTime | `LocalDateTime` | 5.4+ |
| `of(millis)` | 时间戳转 LocalDateTime | `LocalDateTime` | 5.4+ |
| `parse(dateStr)` | 解析字符串 | `LocalDateTime` | 5.4+ |
| `parse(dateStr, format)` | 指定格式解析 | `LocalDateTime` | 5.4+ |
| `format(ldt, format)` | 格式化 | `String` | 5.4+ |
| `offset(ldt, amount, unit)` | 偏移 | `LocalDateTime` | 5.4+ |
| `between(begin, end)` | 日期间隔 | `Duration` | 5.4+ |
| `between(begin, end, unit)` | 指定单位的间隔 | `long` | 5.4+ |
| `beginOfDay(ldt)` | 当天开始 | `LocalDateTime` | 5.4+ |
| `endOfDay(ldt)` | 当天结束 | `LocalDateTime` | 5.4+ |
| `toEpochMilli(ldt)` | 转时间戳（毫秒） | `long` | 5.4+ |
| `isSameDay(ldt1, ldt2)` | 是否同一天 | `boolean` | 5.8+ |

## 详细 API 与示例

### DateUtil — 日期创建与获取

```java
import cn.hutool.core.date.DateUtil;
import cn.hutool.core.date.DateTime;

// 当前时间
DateTime now = DateUtil.date();          // DateTime 对象
String nowStr = DateUtil.now();          // "2024-01-15 14:30:25"
String todayStr = DateUtil.today();      // "2024-01-15"

// 获取日期各部分
int year = DateUtil.year(now);           // 2024
int month = DateUtil.month(now);         // 0 (一月，从0开始！)
int day = DateUtil.dayOfMonth(now);      // 15
int hour = DateUtil.hour(now, true);     // 14 (24小时制)
int week = DateUtil.dayOfWeek(now);      // 2 (周一)
```

> ⚠️ **注意**：`month()` 返回值从 **0** 开始（0=一月），与 `Calendar` 一致。如需 1~12，使用 `DateUtil.month(date) + 1` 或 `DateUtil.monthEnum(date)`。

### DateUtil — 智能解析

```java
import cn.hutool.core.date.DateUtil;

// 自动识别多种日期格式
DateTime d1 = DateUtil.parse("2024-01-15");
DateTime d2 = DateUtil.parse("2024-01-15 14:30:25");
DateTime d3 = DateUtil.parse("2024/01/15");
DateTime d4 = DateUtil.parse("20240115");
DateTime d5 = DateUtil.parse("2024年01月15日");

// 指定格式解析
DateTime d6 = DateUtil.parse("15/01/2024", "dd/MM/yyyy");
```

### DateUtil — 格式化

```java
import cn.hutool.core.date.DateUtil;
import java.util.Date;

Date date = new Date();

DateUtil.formatDateTime(date);                     // "2024-01-15 14:30:25"
DateUtil.formatDate(date);                         // "2024-01-15"
DateUtil.formatTime(date);                         // "14:30:25"
DateUtil.format(date, "yyyy/MM/dd");               // "2024/01/15"
DateUtil.format(date, "yyyy年MM月dd日 HH时mm分");   // "2024年01月15日 14时30分"
```

### DateUtil — 日期边界

```java
import cn.hutool.core.date.DateUtil;
import cn.hutool.core.date.DateTime;

DateTime now = DateUtil.date();

// 当天的开始和结束
DateTime dayStart = DateUtil.beginOfDay(now);   // 2024-01-15 00:00:00
DateTime dayEnd = DateUtil.endOfDay(now);       // 2024-01-15 23:59:59

// 当月的开始和结束
DateTime monthStart = DateUtil.beginOfMonth(now); // 2024-01-01 00:00:00
DateTime monthEnd = DateUtil.endOfMonth(now);     // 2024-01-31 23:59:59

// 当年、当周
DateTime yearStart = DateUtil.beginOfYear(now);
DateTime weekStart = DateUtil.beginOfWeek(now);   // 本周一
```

> 💡 **常见用途**：数据库按天/月查询时，用 `beginOfDay` 和 `endOfDay` 构造查询范围。

### DateUtil — 日期偏移

```java
import cn.hutool.core.date.DateUtil;
import cn.hutool.core.date.DateField;

DateTime now = DateUtil.date();

// 3天后
DateTime threeDaysLater = DateUtil.offsetDay(now, 3);

// 2个月前（负数表示往前）
DateTime twoMonthsAgo = DateUtil.offsetMonth(now, -2);

// 5小时后
DateTime fiveHoursLater = DateUtil.offsetHour(now, 5);

// 通用偏移
DateTime result = DateUtil.offset(now, DateField.MINUTE, 30);  // 30分钟后
```

### DateUtil — 日期差值

```java
import cn.hutool.core.date.DateUtil;
import cn.hutool.core.date.DateUnit;

DateTime begin = DateUtil.parse("2024-01-01");
DateTime end = DateUtil.parse("2024-03-15");

// 天数差
long days = DateUtil.between(begin, end, DateUnit.DAY);      // 74

// 小时差
long hours = DateUtil.between(begin, end, DateUnit.HOUR);    // 1776

// 分钟差
long minutes = DateUtil.between(begin, end, DateUnit.MINUTE);

// 精确天数差（忽略时分秒）
long exactDays = DateUtil.betweenDay(begin, end, true);
```

### DateUtil — 年龄 / 星座 / 生肖

```java
import cn.hutool.core.date.DateUtil;

// 计算年龄
int age = DateUtil.age(DateUtil.parse("1995-06-15"), DateUtil.date());

// 星座
String zodiac = DateUtil.zodiac(6, 15);         // "双子座"

// 生肖
String animal = DateUtil.getChineseZodiac(1995); // "猪"
```

### DateUtil — 计时器

```java
import cn.hutool.core.date.DateUtil;
import cn.hutool.core.date.TimeInterval;

TimeInterval timer = DateUtil.timer();

// ... 执行一些操作 ...
Thread.sleep(1000);

long elapsed = timer.interval();      // 毫秒数
String elapsedStr = timer.intervalPretty(); // "1秒25毫秒" 等可读格式

timer.restart(); // 重新计时
```

### LocalDateTimeUtil — 基本操作

```java
import cn.hutool.core.date.LocalDateTimeUtil;
import java.time.LocalDateTime;
import java.time.Duration;
import java.time.temporal.ChronoUnit;

// 当前时间
LocalDateTime now = LocalDateTimeUtil.now();

// 解析
LocalDateTime ldt = LocalDateTimeUtil.parse("2024-01-15 14:30:25");
LocalDateTime ldt2 = LocalDateTimeUtil.parse("2024/01/15", "yyyy/MM/dd");

// 格式化
String formatted = LocalDateTimeUtil.format(ldt, "yyyy年MM月dd日");

// 偏移
LocalDateTime tomorrow = LocalDateTimeUtil.offset(now, 1, ChronoUnit.DAYS);

// 日期间隔
Duration duration = LocalDateTimeUtil.between(ldt, now);
long days = LocalDateTimeUtil.between(ldt, now, ChronoUnit.DAYS);

// 边界
LocalDateTime dayStart = LocalDateTimeUtil.beginOfDay(now);
LocalDateTime dayEnd = LocalDateTimeUtil.endOfDay(now);

// 转时间戳
long epochMilli = LocalDateTimeUtil.toEpochMilli(now);
```

## 常见问题 FAQ

### Q: DateUtil 线程安全吗？
**A**: 是的。Hutool 内部使用 `FastDateFormat`（类似 Apache Commons 的实现），而不是 `SimpleDateFormat`，因此线程安全。

### Q: parse 能识别哪些日期格式？
**A**: 常见格式均支持：`yyyy-MM-dd`、`yyyy-MM-dd HH:mm:ss`、`yyyy/MM/dd`、`yyyyMMdd`、`yyyy年MM月dd日` 等。无法识别时会抛 `DateException`。

### Q: DateUtil 和 LocalDateTimeUtil 选哪个？
**A**: Java 8+ 项目推荐 `LocalDateTimeUtil`，API 更现代且天然线程安全。但老项目使用 `java.util.Date` 较多时，`DateUtil` 更方便。

### Q: month() 为什么从 0 开始？
**A**: 与 `java.util.Calendar` 保持一致。使用 `monthEnum()` 可获取枚举值。

### Q: between 计算天数差时，是否包含开始日和结束日？
**A**: 不包含（差值计算）。如 1月1日到1月3日差值为 2 天。

## 最佳实践

1. **新项目用 `LocalDateTimeUtil`**：拥抱 Java 8 Time API。
2. **查询日期范围用 `beginOfDay` / `endOfDay`**：避免手写 `00:00:00` 和 `23:59:59`。
3. **耗时统计用 `DateUtil.timer()`**：比手写 `System.currentTimeMillis()` 差值更优雅。
4. **避免直接 `new SimpleDateFormat`**：Hutool 的格式化方法已线程安全。
5. **年龄计算用 `DateUtil.age()`**：自动处理闰年和跨年等边界情况。
