---
title: 集合工具
classes:
  - cn.hutool.core.collection.CollUtil
  - cn.hutool.core.collection.ListUtil
  - cn.hutool.core.map.MapUtil
since: 3.1.0
tags: [集合, collection, list, map, set, CollUtil, ListUtil, MapUtil]
---

# 集合工具 — CollUtil / ListUtil / MapUtil

## 概述

Hutool 提供了三个核心集合工具类：

- **`CollUtil`**：通用集合工具，涵盖 `Collection`、`List`、`Set` 等的创建、判空、运算、转换操作。
- **`ListUtil`**：`List` 专属工具，提供分页、分组、排序等增强操作。
- **`MapUtil`**：`Map` 专属工具，提供类型安全取值、键值操作等功能。

所有方法均为静态方法，对 `null` 有良好的容错处理。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### CollUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `newArrayList(values...)` | 快速创建 ArrayList | `ArrayList<T>` | 3.1+ |
| `newHashSet(values...)` | 快速创建 HashSet | `HashSet<T>` | 3.1+ |
| `newLinkedHashSet(values...)` | 创建 LinkedHashSet | `LinkedHashSet<T>` | 4.1+ |
| `isEmpty(coll)` | 集合判空（null 或 size=0） | `boolean` | 3.1+ |
| `isNotEmpty(coll)` | 集合非空判断 | `boolean` | 3.1+ |
| `union(coll1, coll2)` | 并集 | `Collection<T>` | 3.1+ |
| `intersection(coll1, coll2)` | 交集 | `Collection<T>` | 3.1+ |
| `disjunction(coll1, coll2)` | 差异（对称差集） | `Collection<T>` | 3.1+ |
| `subtract(coll1, coll2)` | 差集（coll1 有 coll2 没有） | `Collection<T>` | 5.6+ |
| `join(coll, separator)` | 集合转字符串 | `String` | 3.1+ |
| `filter(coll, filter)` | 过滤 | `Collection<T>` | 5.4+ |
| `zip(keys, values)` | 两集合组合为 Map | `Map<K,V>` | 4.0+ |
| `addAll(coll, values...)` | 批量添加 | `Collection<T>` | 3.1+ |
| `getFirst(coll)` | 获取首元素 | `T` | 5.7+ |
| `getLast(coll)` | 获取尾元素 | `T` | 5.7+ |
| `sub(list, start, end)` | 安全截取子列表 | `List<T>` | 3.1+ |
| `sortByProperty(list, property)` | 按属性排序 | `List<T>` | 4.0+ |
| `distinct(coll)` | 去重 | `ArrayList<T>` | 5.2+ |
| `contains(coll, value)` | 包含判断 | `boolean` | 3.1+ |
| `containsAny(coll1, coll2)` | 包含任一 | `boolean` | 5.1+ |
| `containsAll(coll1, coll2)` | 包含全部 | `boolean` | 5.1+ |
| `page(pageNo, pageSize, list)` | 分页 | `List<T>` | 5.2+ |
| `reverse(list)` | 反转列表 | `List<T>` | 4.0+ |
| `count(coll, matcher)` | 统计匹配个数 | `int` | 5.5+ |
| `countMap(coll)` | 元素出现次数统计 | `Map<T, Integer>` | 5.2+ |
| `toList(iterable)` | 转 ArrayList | `List<T>` | 3.1+ |
| `max(coll)` | 最大值 | `T` | 5.4+ |
| `min(coll)` | 最小值 | `T` | 5.4+ |

### ListUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `toList(values...)` | 创建 ArrayList | `List<T>` | 5.2+ |
| `of(values...)` | 创建不可变 List（5.7+） | `List<T>` | 5.7+ |
| `empty()` | 空不可变列表 | `List<T>` | 5.2+ |
| `page(pageNo, pageSize, list)` | 分页 | `List<T>` | 5.2+ |
| `sub(list, start, end)` | 截取 | `List<T>` | 5.2+ |
| `sortByProperty(list, property)` | 按属性排序 | `List<T>` | 5.4+ |
| `partition(list, size)` | 分组（每组 size 个） | `List<List<T>>` | 5.4+ |
| `unmodifiable(list)` | 不可变列表 | `List<T>` | 5.2+ |
| `reverse(list)` | 反转 | `List<T>` | 5.2+ |
| `swapTo(list, element, targetIndex)` | 交换到目标位置 | `List<T>` | 5.7+ |
| `swapElement(list, idx1, idx2)` | 交换两个元素 | `List<T>` | 5.7+ |

### MapUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `isEmpty(map)` | Map 判空 | `boolean` | 3.1+ |
| `isNotEmpty(map)` | Map 非空判断 | `boolean` | 3.1+ |
| `newHashMap()` | 创建 HashMap | `HashMap<K,V>` | 3.1+ |
| `of(key, value, ...)` | 快速创建 Map | `Map<K,V>` | 5.2+ |
| `getStr(map, key)` | 类型安全取 String | `String` | 4.0+ |
| `getInt(map, key)` | 类型安全取 Integer | `Integer` | 4.0+ |
| `getLong(map, key)` | 类型安全取 Long | `Long` | 4.0+ |
| `getBool(map, key)` | 类型安全取 Boolean | `Boolean` | 4.0+ |
| `filter(map, filter)` | 过滤 | `Map<K,V>` | 5.4+ |
| `reverse(map)` | 键值互换 | `Map<V,K>` | 4.0+ |
| `join(map, separator, kvSeparator)` | 转字符串 | `String` | 5.0+ |
| `sort(map)` | 排序 | `TreeMap<K,V>` | 4.0+ |
| `toCamelCaseMap(map)` | 键转驼峰 | `Map<String,V>` | 5.2+ |
| `renameKey(map, oldKey, newKey)` | 重命名 key | `Map<K,V>` | 5.6+ |

## 详细 API 与示例

### CollUtil — 创建集合

```java
import cn.hutool.core.collection.CollUtil;

// 快速创建带初始值的集合
ArrayList<String> list = CollUtil.newArrayList("a", "b", "c");
HashSet<String> set = CollUtil.newHashSet("x", "y", "z");

// 从其他集合创建
List<String> list2 = CollUtil.toList(someIterable);
```

### CollUtil — 集合运算

```java
import cn.hutool.core.collection.CollUtil;

Collection<String> a = CollUtil.newArrayList("1", "2", "3");
Collection<String> b = CollUtil.newArrayList("2", "3", "4");

// 并集：[1, 2, 3, 4]
Collection<String> union = CollUtil.union(a, b);

// 交集：[2, 3]
Collection<String> inter = CollUtil.intersection(a, b);

// 对称差集（并集 - 交集）：[1, 4]
Collection<String> disj = CollUtil.disjunction(a, b);

// 差集（a 有 b 没有）：[1]
Collection<String> sub = CollUtil.subtract(a, b);
```

### CollUtil — join / zip

```java
import cn.hutool.core.collection.CollUtil;

// 集合转字符串
List<String> list = CollUtil.newArrayList("a", "b", "c");
String joined = CollUtil.join(list, ",");  // => "a,b,c"

// 两个集合组装为 Map
List<String> keys = CollUtil.newArrayList("name", "age");
List<Object> values = CollUtil.newArrayList("张三", 25);
Map<String, Object> map = CollUtil.zip(keys, values);
// => {name=张三, age=25}
```

### CollUtil — 过滤与统计

```java
import cn.hutool.core.collection.CollUtil;

List<Integer> nums = CollUtil.newArrayList(1, 2, 3, 4, 5, 6);

// 过滤偶数
Collection<Integer> evens = CollUtil.filter(nums, n -> n % 2 == 0);
// => [2, 4, 6]

// 统计匹配个数
int count = CollUtil.count(nums, n -> n > 3);  // => 3

// 元素出现次数统计
List<String> words = CollUtil.newArrayList("a", "b", "a", "c", "a");
Map<String, Integer> countMap = CollUtil.countMap(words);
// => {a=3, b=1, c=1}
```

### CollUtil — 分页与去重

```java
import cn.hutool.core.collection.CollUtil;

List<Integer> list = CollUtil.newArrayList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// 分页（页码从0开始）
List<Integer> page1 = CollUtil.page(0, 3, list);  // => [1, 2, 3]
List<Integer> page2 = CollUtil.page(1, 3, list);  // => [4, 5, 6]

// 去重
List<String> dupList = CollUtil.newArrayList("a", "b", "a", "c");
List<String> distinct = CollUtil.distinct(dupList);  // => ["a", "b", "c"]
```

### ListUtil — 分组 / 分页

```java
import cn.hutool.core.collection.ListUtil;

List<Integer> list = ListUtil.toList(1, 2, 3, 4, 5, 6, 7);

// 每3个一组
List<List<Integer>> groups = ListUtil.partition(list, 3);
// => [[1,2,3], [4,5,6], [7]]

// 分页
List<Integer> page = ListUtil.page(0, 3, list);  // => [1, 2, 3]

// 不可变列表
List<Integer> immutable = ListUtil.unmodifiable(list);
// immutable.add(8); // UnsupportedOperationException!
```

### MapUtil — 创建与取值

```java
import cn.hutool.core.map.MapUtil;

// 快速创建 Map
Map<String, Object> map = MapUtil.of(
    "name", "张三",
    "age", 25,
    "active", true
);

// 类型安全取值（自动转换类型）
String name = MapUtil.getStr(map, "name");    // => "张三"
Integer age = MapUtil.getInt(map, "age");     // => 25
Boolean active = MapUtil.getBool(map, "active"); // => true

// 不存在的 key 返回 null，不会抛异常
String missing = MapUtil.getStr(map, "noKey"); // => null
```

### MapUtil — 过滤与转换

```java
import cn.hutool.core.map.MapUtil;

Map<String, Integer> scores = MapUtil.of("math", 90, "eng", 75, "sci", 85);

// 过滤分数 >= 80 的科目
Map<String, Integer> high = MapUtil.filter(scores,
    entry -> entry.getValue() >= 80);
// => {math=90, sci=85}

// 键值互换
Map<Integer, String> reversed = MapUtil.reverse(scores);
// => {90=math, 75=eng, 85=sci}

// 下划线键转驼峰
Map<String, Object> dbRow = MapUtil.of("user_name", "张三", "create_time", "2024-01-01");
Map<String, Object> camelMap = MapUtil.toCamelCaseMap(dbRow);
// => {userName=张三, createTime=2024-01-01}
```

## 常见问题 FAQ

### Q: CollUtil.isEmpty 和直接用 collection == null || collection.isEmpty() 有什么区别？
**A**: 功能一样，但 `CollUtil.isEmpty` 更简洁且 null 安全，一行搞定两个判断。

### Q: union 返回的是什么类型？会去重吗？
**A**: 返回 `ArrayList`，**不会去重**。如需去重并集，可用 `CollUtil.newHashSet(CollUtil.union(a, b))`。

### Q: CollUtil.page 的页码是从 0 还是 1 开始？
**A**: 默认从 **0** 开始。如果业务中页码从 1 开始，需要 `pageNo - 1` 后再传入。

### Q: partition 和 page 有什么区别？
**A**: `partition` 将整个列表按固定大小分组，返回所有组的列表；`page` 只返回指定页码的那一组。

### Q: MapUtil.of 支持多少对键值？
**A**: `MapUtil.of` 支持 1~10 对键值，超过 10 对建议手动 put。

## 最佳实践

1. **创建集合用 `CollUtil.newArrayList`**：比 `new ArrayList<>(Arrays.asList(...))` 更简洁。
2. **集合运算用 `CollUtil`**：交集、并集、差集等操作不再需要手写循环。
3. **批量处理用 `ListUtil.partition`**：如批量插入数据库时，先分组再逐组处理。
4. **数据库结果集转换用 `MapUtil.toCamelCaseMap`**：将下划线字段名自动转为驼峰命名。
5. **不要在循环中调用 `CollUtil.contains`**：时间复杂度 O(n)，大数据量时先转 `HashSet` 再判断。
