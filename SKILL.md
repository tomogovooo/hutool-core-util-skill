---
name: hutool-core-util-skill
description: >
  Hutool (cn.hutool) 核心模块 (hutool-core) 工具类速查与最佳实践 Skill。
  当用户在 Java/Kotlin 项目中需要进行字符串处理、集合操作、日期时间、Bean 拷贝、
  类型转换、文件IO、正则、ID 生成、数字运算等常见操作时，本 Skill 提供
  Hutool 对应工具类的 API 速查、示例代码和最佳实践建议。
---

# Hutool Core Util Skill

> **版本适用范围**：Hutool 5.x / 6.x（hutool-core 模块）
> **语言**：Java / Kotlin

---

## 1. Skill 概述

本 Skill 是 **Hutool 核心工具类的结构化知识库**。它将 `hutool-core` 中最常用的
十余个工具类按功能分门别类，提供：

- ✅ API 签名速查
- ✅ 典型使用示例
- ✅ 常见陷阱与最佳实践
- ✅ 与 JDK / Apache Commons / Guava 的对比参考

当用户提出与以下场景相关的编码问题时，**必须激活本 Skill** 并路由到对应的知识文档。

---

## 2. 触发词 → 知识文档路由表

根据用户输入中的 **关键词 / 意图**，路由到 `data/` 目录下对应的文档。
匹配时采用 **优先最长匹配** 原则；若用户问题跨多个分类，依次查阅所有相关文档。

| # | 触发关键词 / 意图 | 路由文档 | 核心工具类 |
|---|---|---|---|
| 01 | `字符串`、`StrUtil`、`字符串判空`、`trim`、`split`、`join`、`format`、`sub`、`contains`、`startWith`、`endWith`、`removePrefix`、`removeSuffix`、`toUnderlineCase`、`toCamelCase`、`CharSequenceUtil`、`isBlank`、`isEmpty`、`nullToEmpty`、`repeat`、`padPre`、`padAfter`、`brief` | [01-string-util.md](data/01-string-util.md) | `StrUtil`, `CharSequenceUtil` |
| 02 | `集合`、`List`、`Map`、`Set`、`CollUtil`、`ListUtil`、`MapUtil`、`集合判空`、`newArrayList`、`newHashMap`、`filter`、`zip`、`unzip`、`sortByProperty`、`page`、`sub`、`partition`、`reverse`、`union`、`intersection`、`disjunction`、`subtract` | [02-collection-util.md](data/02-collection-util.md) | `CollUtil`, `ListUtil`, `MapUtil` |
| 03 | `对象`、`Bean`、`BeanUtil`、`ObjectUtil`、`isNull`、`isNotNull`、`equal`、`clone`、`defaultIfNull`、`copyProperties`、`beanToMap`、`mapToBean`、`fillBean`、`BeanDesc`、`属性拷贝`、`对象转换` | [03-object-bean-util.md](data/03-object-bean-util.md) | `ObjectUtil`, `BeanUtil` |
| 04 | `日期`、`时间`、`Date`、`DateTime`、`DateUtil`、`LocalDateTime`、`LocalDateTimeUtil`、`now`、`parse`、`format`、`offset`、`between`、`beginOfDay`、`endOfDay`、`age`、`isLeapYear`、`zodiac`、`formatDateTime`、`日期格式化`、`时间差` | [04-date-time-util.md](data/04-date-time-util.md) | `DateUtil`, `LocalDateTimeUtil` |
| 05 | `数字`、`数学`、`NumberUtil`、`MathUtil`、`add`、`sub`、`mul`、`div`、`round`、`decimalFormat`、`isNumber`、`isInteger`、`parseInt`、`BigDecimal`、`精度`、`四舍五入`、`金额`、`计算` | [05-number-math-util.md](data/05-number-math-util.md) | `NumberUtil`, `MathUtil` |
| 06 | `文件`、`IO`、`流`、`File`、`FileUtil`、`IoUtil`、`ResourceUtil`、`readUtf8String`、`writeUtf8String`、`copy`、`del`、`mkdir`、`ls`、`readLines`、`getReader`、`getWriter`、`close`、`resource`、`classpath`、`文件读写`、`文件复制`、`流操作` | [06-io-file-util.md](data/06-io-file-util.md) | `IoUtil`, `FileUtil`, `ResourceUtil` |
| 07 | `转换`、`Convert`、`类型转换`、`toStr`、`toInt`、`toLong`、`toDouble`、`toBool`、`toList`、`toMap`、`toDate`、`convertQuietly`、`半角全角`、`hex`、`unicode` | [07-convert-util.md](data/07-convert-util.md) | `Convert` |
| 08 | `ID`、`UUID`、`雪花`、`Snowflake`、`IdUtil`、`RandomUtil`、`randomUUID`、`simpleUUID`、`objectId`、`nanoId`、`randomInt`、`randomEle`、`randomString`、`getSnowflake`、`随机数`、`唯一ID` | [08-id-random-util.md](data/08-id-random-util.md) | `IdUtil`, `RandomUtil` |
| 09 | `正则`、`Regex`、`ReUtil`、`isMatch`、`getGroup`、`findAll`、`replaceAll`、`escape`、`Pattern`、`正则匹配`、`正则提取`、`正则替换` | [09-regex-util.md](data/09-regex-util.md) | `ReUtil` |
| 10 | `数组`、`Array`、`ArrayUtil`、`枚举`、`Enum`、`EnumUtil`、`isEmpty`、`isNotEmpty`、`newArray`、`resize`、`addAll`、`contains`、`indexOf`、`wrap`、`unWrap`、`getNames`、`getFieldValues`、`likeValueOf` | [10-array-enum-util.md](data/10-array-enum-util.md) | `ArrayUtil`, `EnumUtil` |
| 11 | `反射`、`Class`、`ClassUtil`、`ReflectUtil`、`getClassName`、`getMethod`、`invoke`、`getFieldValue`、`setFieldValue`、`newInstance`、`scanPackage`、`isAssignable`、`类加载`、`反射调用` | [11-class-reflection-util.md](data/11-class-reflection-util.md) | `ClassUtil`, `ReflectUtil` |
| 12 | `网络`、`Net`、`NetUtil`、`IP`、`ipv4`、`ping`、`isInnerIP`、`localHostName`、`getLocalhostStr`、`longToIpv4`、`isUsableLocalPort`、`端口`、`IP地址` | [12-network-util.md](data/12-network-util.md) | `NetUtil` |
| 13 | `分页`、`PageUtil`、`压缩`、`Zip`、`ZipUtil`、`URL`、`URLUtil`、`Hex`、`HexUtil`、`Hash`、`HashUtil`、`Escape`、`EscapeUtil`、`CharUtil`、`BooleanUtil`、`DesensitizedUtil`、`脱敏`、`信息脱敏` | [13-other-util.md](data/13-other-util.md) | `PageUtil`, `ZipUtil`, 其他 |

---

## 3. 使用流程

当激活本 Skill 后，请严格按以下流程处理用户请求：

### 3.1 意图识别与路由

```
用户输入
  │
  ├─ 提取关键词 / 识别编码意图
  │
  ├─ 匹配「触发词路由表」（第 2 节）
  │     ├─ 命中单个分类 → 读取对应 data/*.md
  │     ├─ 命中多个分类 → 依次读取所有相关 data/*.md
  │     └─ 未命中 → 尝试模糊匹配类名/方法名，或提示用户明确需求
  │
  └─ 进入回答生成阶段
```

### 3.2 回答生成规范

1. **优先使用知识文档中的内容**：API 签名、示例代码、注意事项均以 `data/` 文档为准。
2. **给出可直接运行的代码示例**：包含必要的 `import` 语句，标注 Hutool 版本要求（如有差异）。
3. **标注常见陷阱**：如空指针风险、性能注意点、线程安全性等。
4. **提供替代方案**（当合适时）：对比 JDK 原生 API 或其他常见库的实现方式。
5. **尊重用户偏好**：若 `memory.md` 中记录了用户的编码风格偏好（如倾向链式调用、偏好 Kotlin 语法等），在生成代码时予以遵从。

### 3.3 回答模板

```markdown
## [工具类名].[方法名] — [一句话功能描述]

**所属模块**：hutool-core
**最低版本**：5.x.x（如有特殊要求）

### 方法签名
​```java
public static [返回类型] [方法名]([参数列表])
​```

### 使用示例
​```java
import cn.hutool.core.util.StrUtil;

// 示例描述
String result = StrUtil.format("Hello, {}!", "Hutool");
// => "Hello, Hutool!"
​```

### ⚠️ 注意事项
- [陷阱或最佳实践说明]

### 🔗 相关方法
- `StrUtil.isBlank()` — 判断字符串是否为空白
- `StrUtil.emptyToNull()` — 空字符串转为 null
```

---

## 4. 项目级自定义

如果用户的项目有特定的 Hutool 使用规范（如封装了统一的工具类、限制了某些 API 的使用），
可在 `project/` 目录下创建项目专属的配置文档。

### 使用方式

1. 在 `project/` 下创建 `<项目名>.md`。
2. 在文档中声明项目的 Hutool 版本、禁用/推荐的 API、封装约定等。
3. 回答时会优先参考项目级规范，然后再参考通用知识文档。

---

## 5. 用户偏好（memory.md）

`memory.md` 记录用户的个性化偏好，例如：

- 偏好的代码风格（链式 / 传统）
- 偏好的语言（Java / Kotlin）
- 是否需要显示 JDK 对比方案
- Hutool 版本锁定

当 `memory.md` 存在时，在生成回答前先读取其内容并遵从配置。

---

## 6. 知识库文件规范

`data/` 目录下的每个文档应遵循以下结构：

```markdown
---
title: [工具类名称]
classes: [涉及的类全限定名列表]
since: [最早可用的 Hutool 版本]
tags: [分类标签]
---

# [工具类名称]

## 概述
[工具类的定位与核心功能简述]

## Maven 依赖
[引入方式]

## 常用方法速查表
| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|

## 详细 API 与示例
### [方法名]
- 签名
- 参数说明
- 返回值
- 示例代码
- 注意事项

## 常见问题 FAQ

## 最佳实践
```

---

## 7. 注意事项

1. **版本差异**：Hutool 5.x 与 6.x 在部分 API 上有 breaking changes（如包名从 `cn.hutool` 变为 `org.dromara.hutool`）。回答时务必确认用户使用的版本。
2. **性能考量**：某些工具方法（如 `BeanUtil.copyProperties` 使用反射）在高频场景下需注意性能，应在回答中适当提醒。
3. **线程安全**：`DateUtil` 中涉及 `SimpleDateFormat` 的方法并非线程安全，推荐使用 `LocalDateTimeUtil` 或 Hutool 的 `FastDateFormat`。
4. **空值处理**：Hutool 大部分工具方法对 `null` 有良好的容错处理，但仍需根据具体方法说明是否会抛出异常。
