---
title: 对象与Bean工具
classes:
  - cn.hutool.core.util.ObjectUtil
  - cn.hutool.core.bean.BeanUtil
since: 3.0.0
tags: [对象, object, bean, 拷贝, copy, 属性, BeanUtil, ObjectUtil]
---

# 对象与Bean工具 — ObjectUtil / BeanUtil

## 概述

- **`ObjectUtil`**：通用对象工具类，提供 null 安全的比较、克隆、默认值、类型判断等操作。
- **`BeanUtil`**：JavaBean 工具类，提供属性拷贝（`copyProperties`）、Bean 与 Map 互转、属性存取等功能。是业务开发中 DO/DTO/VO 转换的利器。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### ObjectUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `isNull(obj)` | 判断是否为 null | `boolean` | 3.0+ |
| `isNotNull(obj)` | 判断是否不为 null | `boolean` | 3.0+ |
| `isEmpty(obj)` | 判断是否为"空"（null/空串/空集合/空数组） | `boolean` | 5.0+ |
| `isNotEmpty(obj)` | 判断是否不为"空" | `boolean` | 5.0+ |
| `equal(a, b)` | null 安全的 equals 比较 | `boolean` | 3.0+ |
| `notEqual(a, b)` | null 安全的不相等判断 | `boolean` | 5.4+ |
| `defaultIfNull(obj, defaultValue)` | null 时返回默认值 | `T` | 3.0+ |
| `defaultIfEmpty(obj, defaultValue)` | 空时返回默认值 | `T` | 5.7+ |
| `defaultIfBlank(str, defaultValue)` | 空白时返回默认值 | `String` | 5.7+ |
| `clone(obj)` | 克隆对象 | `T` | 3.0+ |
| `cloneByStream(obj)` | 通过序列化深克隆 | `T` | 3.0+ |
| `isBasicType(clazz)` | 判断是否基本类型 | `boolean` | 3.0+ |
| `serialize(obj)` | 对象序列化为字节数组 | `byte[]` | 3.0+ |
| `deserialize(bytes)` | 字节数组反序列化为对象 | `T` | 3.0+ |
| `length(obj)` | 获取长度（字符串/集合/数组） | `int` | 5.5+ |
| `compare(a, b)` | null 安全的 Comparable 比较 | `int` | 5.4+ |

### BeanUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `copyProperties(source, target)` | 属性拷贝 | `void` | 3.0+ |
| `copyProperties(source, target, copyOptions)` | 带选项的属性拷贝 | `void` | 5.4+ |
| `copyToList(sourceList, targetClass)` | 批量拷贝为列表 | `List<T>` | 5.5+ |
| `beanToMap(bean)` | Bean 转 Map | `Map<String, Object>` | 3.0+ |
| `beanToMap(bean, isToUnderlineCase, ignoreNull)` | 带选项的 Bean 转 Map | `Map<String, Object>` | 5.0+ |
| `mapToBean(map, beanClass)` | Map 转 Bean | `T` | 3.0+ |
| `fillBean(bean, valueProvider)` | 填充 Bean 属性 | `T` | 3.0+ |
| `fillBeanWithMap(map, bean, ignoreCase)` | Map 填充 Bean | `T` | 3.0+ |
| `getProperty(bean, expression)` | 获取属性值（支持嵌套） | `Object` | 5.0+ |
| `setProperty(bean, expression, value)` | 设置属性值（支持嵌套） | `void` | 5.0+ |
| `isEmpty(bean, ignoreFields...)` | Bean 是否所有属性为空 | `boolean` | 5.7+ |
| `isNotEmpty(bean, ignoreFields...)` | Bean 是否有非空属性 | `boolean` | 5.7+ |
| `hasNullField(bean)` | 是否有 null 字段 | `boolean` | 5.7+ |
| `getBeanDesc(beanClass)` | 获取 Bean 描述信息 | `BeanDesc` | 5.0+ |
| `trimStrFields(bean)` | 去除所有 String 字段的首尾空白 | `T` | 5.7+ |

## 详细 API 与示例

### ObjectUtil — null 安全操作

```java
import cn.hutool.core.util.ObjectUtil;

// null 判断
ObjectUtil.isNull(null);        // true
ObjectUtil.isNotNull("hello");  // true

// null 安全的 equals
ObjectUtil.equal(null, null);   // true
ObjectUtil.equal("a", "a");     // true
ObjectUtil.equal(null, "a");    // false（不会 NPE）

// 默认值
String name = ObjectUtil.defaultIfNull(getName(), "匿名用户");
String value = ObjectUtil.defaultIfBlank(getInput(), "默认值");
```

### ObjectUtil — 克隆

```java
import cn.hutool.core.util.ObjectUtil;

// 浅克隆（对象需实现 Cloneable）
MyObject cloned = ObjectUtil.clone(originalObj);

// 深克隆（对象需实现 Serializable）
MyObject deepCloned = ObjectUtil.cloneByStream(originalObj);
```

> ⚠️ **注意**：`cloneByStream` 通过序列化实现深克隆，性能较低，不适合高频调用场景。

### ObjectUtil — 通用判空

```java
import cn.hutool.core.util.ObjectUtil;

// isEmpty 可判断多种类型的"空"
ObjectUtil.isEmpty(null);               // true
ObjectUtil.isEmpty("");                 // true
ObjectUtil.isEmpty(new ArrayList<>());  // true
ObjectUtil.isEmpty(new int[0]);         // true
ObjectUtil.isEmpty(" ");               // false（空格不算空）
```

### BeanUtil.copyProperties — 属性拷贝

```java
import cn.hutool.core.bean.BeanUtil;
import cn.hutool.core.bean.copier.CopyOptions;

// 基础拷贝
UserDO source = new UserDO("张三", 25, "test@test.com");
UserVO target = new UserVO();
BeanUtil.copyProperties(source, target);
// target 中与 source 同名属性被赋值

// 忽略 null 值
CopyOptions options = CopyOptions.create()
    .setIgnoreNullValue(true);
BeanUtil.copyProperties(source, target, options);

// 忽略错误（类型不匹配时不抛异常）
CopyOptions options2 = CopyOptions.create()
    .setIgnoreError(true);
BeanUtil.copyProperties(source, target, options2);

// 忽略大小写
CopyOptions options3 = CopyOptions.create()
    .setIgnoreCase(true);

// 字段映射（源字段名 → 目标字段名）
CopyOptions options4 = CopyOptions.create()
    .setFieldMapping(MapUtil.of("userName", "name"));

// 组合使用
CopyOptions fullOptions = CopyOptions.create()
    .setIgnoreNullValue(true)
    .setIgnoreError(true)
    .setIgnoreCase(true)
    .setFieldMapping(MapUtil.of("userName", "name"));
BeanUtil.copyProperties(source, target, fullOptions);
```

#### CopyOptions 常用配置一览

| 配置方法 | 功能 | 默认值 |
|---------|------|--------|
| `setIgnoreNullValue(true)` | 忽略源对象中为 null 的属性 | `false` |
| `setIgnoreError(true)` | 类型不匹配时忽略，不抛异常 | `false` |
| `setIgnoreCase(true)` | 属性名匹配忽略大小写 | `false` |
| `setFieldMapping(map)` | 字段名映射（源 → 目标） | 空 |
| `setIgnoreProperties(names...)` | 忽略指定属性不拷贝 | 空 |
| `setOverride(false)` | 目标属性非 null 时不覆盖 | `true` |
| `setPropertiesFilter(filter)` | 自定义属性过滤器 | 无 |
| `setFieldValueEditor(editor)` | 字段值编辑器 | 无 |

### BeanUtil.copyToList — 批量拷贝

```java
import cn.hutool.core.bean.BeanUtil;

// DO 列表批量转 VO 列表
List<UserDO> doList = userMapper.selectList();
List<UserVO> voList = BeanUtil.copyToList(doList, UserVO.class);
```

### BeanUtil — Bean 与 Map 互转

```java
import cn.hutool.core.bean.BeanUtil;

// Bean → Map
UserDO user = new UserDO("张三", 25);
Map<String, Object> map = BeanUtil.beanToMap(user);
// => {name=张三, age=25}

// Bean → Map（下划线键名，忽略 null）
Map<String, Object> map2 = BeanUtil.beanToMap(user, true, true);
// => {name=张三, age=25}  键名变为 user_name 等

// Map → Bean
Map<String, Object> dataMap = new HashMap<>();
dataMap.put("name", "李四");
dataMap.put("age", 30);
UserDO newUser = BeanUtil.mapToBean(dataMap, UserDO.class, false);

// Map 填充已有 Bean（忽略大小写）
BeanUtil.fillBeanWithMap(dataMap, existingUser, true);
```

### BeanUtil — 属性存取（支持嵌套表达式）

```java
import cn.hutool.core.bean.BeanUtil;

// 获取嵌套属性
Object city = BeanUtil.getProperty(user, "address.city");

// 设置嵌套属性
BeanUtil.setProperty(user, "address.city", "北京");

// 获取 List 中元素的属性
Object firstFriendName = BeanUtil.getProperty(user, "friends[0].name");
```

### BeanUtil — Bean 判空与处理

```java
import cn.hutool.core.bean.BeanUtil;

// 判断所有属性是否为空
boolean empty = BeanUtil.isEmpty(user);

// 判断是否有 null 字段
boolean hasNull = BeanUtil.hasNullField(user);

// 去除所有 String 字段的首尾空白
BeanUtil.trimStrFields(user);
// user.getName() 中的 "  张三  " → "张三"
```

## 常见问题 FAQ

### Q: copyProperties 是深拷贝还是浅拷贝？
**A**: **浅拷贝**。引用类型字段只拷贝引用。如需深拷贝，先 `ObjectUtil.cloneByStream` 或使用 JSON 序列化方式。

### Q: copyProperties 性能怎么样？
**A**: 底层基于反射，性能弱于手写 setter。低频调用没问题，**高频场景**（如循环中每次都 copy）建议使用 MapStruct 等编译期方案。

### Q: 源和目标字段名不一样怎么办？
**A**: 使用 `CopyOptions.setFieldMapping(Map)` 配置字段映射关系。

### Q: beanToMap 转出来的 key 是驼峰还是下划线？
**A**: 默认驼峰。使用 `beanToMap(bean, true, false)` 第二个参数为 `true` 可转为下划线格式。

### Q: getProperty 支持哪些表达式？
**A**: 支持点号嵌套 `address.city`、数组索引 `friends[0]`、Map 键 `extra[key]` 等。

## 最佳实践

1. **DO/DTO/VO 转换用 `copyProperties`**：简单场景足够；复杂映射配合 `CopyOptions`。
2. **批量转换用 `copyToList`**：一行代码替代循环拷贝。
3. **务必配置 `ignoreNullValue`**：更新操作中，避免 null 值覆盖已有数据。
4. **高频场景考虑替代方案**：MapStruct（编译期生成代码，零反射开销）。
5. **Bean 入库前用 `trimStrFields`**：避免前端传入的多余空格污染数据。
6. **null 比较用 `ObjectUtil.equal`**：避免 `a.equals(b)` 的 NPE 风险。
