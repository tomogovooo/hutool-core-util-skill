---
title: 数组与枚举工具
classes:
  - cn.hutool.core.util.ArrayUtil
  - cn.hutool.core.util.EnumUtil
since: 3.0.0
tags: [数组, array, 枚举, enum, ArrayUtil, EnumUtil]
---

# 数组与枚举工具 — ArrayUtil / EnumUtil

## 概述

- **`ArrayUtil`**：数组工具类，提供数组的创建、判空、添加、删除、查找、截取、排序、去重、转换等全套操作。对 `null` 数组有良好容错。
- **`EnumUtil`**：枚举工具类，提供枚举的名称获取、字段值获取、模糊匹配等反射操作。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### ArrayUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `isEmpty(array)` | 数组判空（null 或 length=0） | `boolean` | 3.0+ |
| `isNotEmpty(array)` | 数组非空判断 | `boolean` | 3.0+ |
| `newArray(componentType, size)` | 创建泛型数组 | `T[]` | 3.0+ |
| `resize(array, newSize)` | 调整数组大小 | `T[]` | 3.0+ |
| `addAll(arrays...)` | 合并多个数组 | `T[]` | 3.0+ |
| `append(array, elements...)` | 追加元素 | `T[]` | 3.0+ |
| `insert(array, index, elements...)` | 插入元素 | `T[]` | 5.4+ |
| `remove(array, index)` | 移除指定位置元素 | `T[]` | 3.0+ |
| `removeEle(array, element)` | 移除指定元素 | `T[]` | 5.4+ |
| `contains(array, value)` | 是否包含元素 | `boolean` | 3.0+ |
| `containsIgnoreCase(array, value)` | 忽略大小写包含 | `boolean` | 5.4+ |
| `indexOf(array, value)` | 查找元素索引 | `int` | 3.0+ |
| `lastIndexOf(array, value)` | 查找最后出现索引 | `int` | 3.0+ |
| `wrap(primitiveArray)` | 基本类型数组→包装类数组 | `T[]` | 3.0+ |
| `unWrap(wrapperArray)` | 包装类数组→基本类型数组 | `原始类型[]` | 3.0+ |
| `isArray(obj)` | 是否为数组 | `boolean` | 3.0+ |
| `join(array, separator)` | 数组转字符串 | `String` | 3.0+ |
| `zip(keys, values)` | 两数组合并为 Map | `Map<K,V>` | 4.0+ |
| `toArray(collection, componentType)` | 集合转数组 | `T[]` | 3.0+ |
| `sub(array, start, end)` | 截取子数组 | `T[]` | 3.0+ |
| `max(array)` | 最大值 | `T` | 5.4+ |
| `min(array)` | 最小值 | `T` | 5.4+ |
| `distinct(array)` | 去重 | `T[]` | 5.2+ |
| `reverse(array)` | 反转 | `T[]` | 3.0+ |
| `filter(array, filter)` | 过滤 | `T[]` | 5.4+ |
| `map(array, mapper, targetType)` | 映射转换 | `R[]` | 5.4+ |
| `get(array, index)` | 安全获取（支持负索引） | `T` | 5.4+ |
| `swap(array, idx1, idx2)` | 交换元素 | `T[]` | 5.4+ |
| `sort(array)` | 排序 | `T[]` | 3.0+ |
| `isSorted(array)` | 是否已排序 | `boolean` | 5.4+ |
| `hasNull(array)` | 是否含 null | `boolean` | 5.4+ |
| `isAllNull(array)` | 是否全为 null | `boolean` | 5.7+ |
| `isAllNotNull(array)` | 是否全非 null | `boolean` | 5.7+ |
| `firstNonNull(array)` | 第一个非 null 元素 | `T` | 5.4+ |

### EnumUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `getNames(enumClass)` | 获取所有枚举名称 | `List<String>` | 4.0+ |
| `getFieldValues(enumClass, fieldName)` | 获取指定字段的所有值 | `List<Object>` | 5.0+ |
| `getBy(enumClass, condition)` | 按条件获取枚举 | `Enum` | 5.5+ |
| `getFieldBy(enumClass, fieldCondition, fieldName)` | 按条件获取字段值 | `Object` | 5.5+ |
| `likeValueOf(enumClass, value)` | 模糊匹配枚举值 | `Enum` | 5.0+ |
| `getEnumMap(enumClass)` | 枚举转 Map（name→instance） | `Map<String, Enum>` | 4.0+ |
| `contains(enumClass, name)` | 是否包含指定名称 | `boolean` | 5.4+ |

## 详细 API 与示例

### ArrayUtil — 创建与判空

```java
import cn.hutool.core.util.ArrayUtil;

// 判空
ArrayUtil.isEmpty(null);            // true
ArrayUtil.isEmpty(new int[0]);      // true
ArrayUtil.isEmpty(new String[]{});  // true
ArrayUtil.isNotEmpty(new int[]{1}); // true

// 创建泛型数组
String[] arr = ArrayUtil.newArray(String.class, 10);
```

### ArrayUtil — 添加与删除

```java
import cn.hutool.core.util.ArrayUtil;

String[] arr = {"a", "b", "c"};

// 追加元素
String[] appended = ArrayUtil.append(arr, "d", "e");
// => ["a", "b", "c", "d", "e"]

// 插入元素
String[] inserted = ArrayUtil.insert(arr, 1, "x");
// => ["a", "x", "b", "c"]

// 合并多个数组
String[] merged = ArrayUtil.addAll(new String[]{"1"}, new String[]{"2"}, new String[]{"3"});
// => ["1", "2", "3"]

// 移除指定位置
String[] removed = ArrayUtil.remove(arr, 1);
// => ["a", "c"]

// 移除指定元素
String[] removed2 = ArrayUtil.removeEle(arr, "b");
// => ["a", "c"]
```

### ArrayUtil — 查找与包含

```java
import cn.hutool.core.util.ArrayUtil;

String[] arr = {"apple", "banana", "cherry"};

// 包含判断
ArrayUtil.contains(arr, "banana");              // true
ArrayUtil.containsIgnoreCase(arr, "APPLE");     // true

// 查找索引
ArrayUtil.indexOf(arr, "cherry");               // 2
ArrayUtil.indexOf(arr, "grape");                // -1

// 安全获取（支持负索引）
ArrayUtil.get(arr, 0);    // "apple"
ArrayUtil.get(arr, -1);   // "cherry"（倒数第一个）
ArrayUtil.get(arr, 10);   // null（越界不抛异常）
```

### ArrayUtil — 截取、去重、排序

```java
import cn.hutool.core.util.ArrayUtil;

Integer[] nums = {3, 1, 4, 1, 5, 9, 2, 6, 5};

// 截取子数组
Integer[] sub = ArrayUtil.sub(nums, 2, 5);
// => [4, 1, 5]

// 去重
Integer[] unique = ArrayUtil.distinct(nums);
// => [3, 1, 4, 5, 9, 2, 6]

// 排序
Integer[] sorted = ArrayUtil.sort(nums.clone());
// => [1, 1, 2, 3, 4, 5, 5, 6, 9]

// 反转
Integer[] reversed = ArrayUtil.reverse(new Integer[]{1, 2, 3});
// => [3, 2, 1]

// 判断排序状态
ArrayUtil.isSorted(new int[]{1, 2, 3, 4});  // true
```

### ArrayUtil — 包装类型转换

```java
import cn.hutool.core.util.ArrayUtil;

// 基本类型 → 包装类型
int[] primitiveArr = {1, 2, 3};
Integer[] wrapperArr = ArrayUtil.wrap(primitiveArr);

// 包装类型 → 基本类型
Integer[] wrappers = {4, 5, 6};
int[] primitives = ArrayUtil.unWrap(wrappers);
```

### ArrayUtil — 过滤与映射

```java
import cn.hutool.core.util.ArrayUtil;

Integer[] nums = {1, 2, 3, 4, 5, 6};

// 过滤偶数
Integer[] evens = ArrayUtil.filter(nums, n -> n % 2 == 0);
// => [2, 4, 6]

// 映射转换（Integer → String）
String[] strs = ArrayUtil.map(nums, String.class, String::valueOf);
// => ["1", "2", "3", "4", "5", "6"]
```

### ArrayUtil — Null 检查

```java
import cn.hutool.core.util.ArrayUtil;

Object[] arr = {"a", null, "c", null};

ArrayUtil.hasNull(arr);        // true
ArrayUtil.isAllNull(arr);      // false
ArrayUtil.isAllNotNull(arr);   // false

// 获取第一个非 null 元素
Object first = ArrayUtil.firstNonNull(null, null, "hello", "world");
// => "hello"
```

### EnumUtil — 枚举操作

```java
import cn.hutool.core.util.EnumUtil;

// 定义枚举
enum Status {
    ACTIVE("启用", 1),
    INACTIVE("禁用", 0),
    DELETED("删除", -1);

    private final String label;
    private final int code;

    Status(String label, int code) {
        this.label = label;
        this.code = code;
    }
    public String getLabel() { return label; }
    public int getCode() { return code; }
}

// 获取所有枚举名称
List<String> names = EnumUtil.getNames(Status.class);
// => ["ACTIVE", "INACTIVE", "DELETED"]

// 获取指定字段的所有值
List<Object> labels = EnumUtil.getFieldValues(Status.class, "label");
// => ["启用", "禁用", "删除"]

// 枚举转 Map
Map<String, Status> map = EnumUtil.getEnumMap(Status.class);
// => {ACTIVE=ACTIVE, INACTIVE=INACTIVE, DELETED=DELETED}

// 是否包含
EnumUtil.contains(Status.class, "ACTIVE");     // true
EnumUtil.contains(Status.class, "UNKNOWN");    // false

// 模糊匹配（忽略大小写和特殊字符）
Status s = EnumUtil.likeValueOf(Status.class, "active");   // Status.ACTIVE
Status s2 = EnumUtil.likeValueOf(Status.class, "Active");  // Status.ACTIVE

// 按条件获取
Status found = EnumUtil.getBy(Status.class,
    e -> e.getCode() == 1);
// => Status.ACTIVE
```

## 常见问题 FAQ

### Q: ArrayUtil 对 null 数组怎么处理？
**A**: 大部分方法对 null 数组有容错。`isEmpty(null)` 返回 true，`contains(null, x)` 返回 false，`get(null, 0)` 返回 null，不会抛 NPE。

### Q: ArrayUtil.sub 和 Arrays.copyOfRange 的区别？
**A**: `ArrayUtil.sub` 支持负数索引（倒数），越界自动修正，null 安全。

### Q: EnumUtil.likeValueOf 是怎么匹配的？
**A**: 忽略大小写，移除空格、下划线、中划线等特殊字符后比较。`"active"`、`"ACTIVE"`、`"Active"` 都能匹配到 `ACTIVE`。

### Q: 如何根据枚举的某个字段值反查枚举？
**A**: 使用 `EnumUtil.getBy(enumClass, e -> e.getField().equals(value))`。

## 最佳实践

1. **数组判空用 `ArrayUtil.isEmpty`**：一行搞定 null 和 length 判断。
2. **基本类型数组转包装类用 `wrap`**：方便使用 Stream API 等泛型操作。
3. **安全取值用 `ArrayUtil.get`**：支持负索引，越界返回 null 不抛异常。
4. **枚举反查用 `EnumUtil.getBy`**：替代 for 循环遍历枚举值。
5. **前端传参匹配枚举用 `likeValueOf`**：容忍大小写不一致。
6. **空值优先取用 `firstNonNull`**：多个候选值中取第一个非空。
