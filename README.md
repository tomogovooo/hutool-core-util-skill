# Hutool Core Util Skill

> 🛠️ 一个为 AI 编码助手设计的 Hutool 核心工具类知识技能包

## 📖 简介

**hutool-core-util-skill** 是一个结构化的 Skill 知识库，帮助 AI 编码助手在 Java/Kotlin 项目中快速、准确地推荐和使用 [Hutool](https://hutool.cn/) 核心模块（`hutool-core`）的工具类 API。

当你在编码过程中遇到字符串处理、集合操作、日期格式化、Bean 拷贝、文件读写等常见场景时，本 Skill 会自动路由到对应的知识文档，提供 **API 速查、示例代码和最佳实践建议**。

## ✨ 功能特性

- 🔍 **智能路由**：根据关键词 / 编码意图自动匹配对应工具类文档
- 📚 **13 大分类**：覆盖 `hutool-core` 中最常用的工具类
- 💡 **即用示例**：每个 API 附带可直接运行的代码片段
- ⚠️ **陷阱提醒**：标注空指针、线程安全、性能等常见问题
- 🔄 **版本适配**：同时覆盖 Hutool 5.x 和 6.x
- 🎯 **项目定制**：支持项目级规范和用户个性化偏好

## 📦 覆盖范围

| # | 分类 | 核心工具类 | 知识文档 |
|---|------|-----------|----------|
| 01 | 字符串工具 | `StrUtil`, `CharSequenceUtil` | `data/01-string-util.md` |
| 02 | 集合工具 | `CollUtil`, `ListUtil`, `MapUtil` | `data/02-collection-util.md` |
| 03 | 对象与Bean工具 | `ObjectUtil`, `BeanUtil` | `data/03-object-bean-util.md` |
| 04 | 日期时间工具 | `DateUtil`, `LocalDateTimeUtil` | `data/04-date-time-util.md` |
| 05 | 数字与数学工具 | `NumberUtil`, `MathUtil` | `data/05-number-math-util.md` |
| 06 | IO与文件工具 | `IoUtil`, `FileUtil`, `ResourceUtil` | `data/06-io-file-util.md` |
| 07 | 类型转换工具 | `Convert` | `data/07-convert-util.md` |
| 08 | ID与随机数工具 | `IdUtil`, `RandomUtil` | `data/08-id-random-util.md` |
| 09 | 正则表达式工具 | `ReUtil` | `data/09-regex-util.md` |
| 10 | 数组与枚举工具 | `ArrayUtil`, `EnumUtil` | `data/10-array-enum-util.md` |
| 11 | 类与反射工具 | `ClassUtil`, `ReflectUtil` | `data/11-class-reflection-util.md` |
| 12 | 网络工具 | `NetUtil` | `data/12-network-util.md` |
| 13 | 其他常用工具 | `PageUtil`, `ZipUtil`, `URLUtil` 等 | `data/13-other-util.md` |

## 🚀 安装与使用

### 1. 安装 Skill

将本目录放置到你的 AI 编码助手的 Skill 目录下：

```
<skills-directory>/
  └── hutool-core-util-skill/
        ├── SKILL.md
        ├── README.md
        ├── memory.md
        ├── project/
        └── data/
```

### 2. 自动激活

无需手动操作。当你在对话中提到 Hutool 工具类相关的关键词时，Skill 会自动激活并路由到对应文档。

**触发示例**：

```
"帮我用 StrUtil 格式化字符串"
"Hutool 怎么生成雪花ID？"
"BeanUtil.copyProperties 怎么忽略空值？"
"集合取交集用什么方法？"
"DateUtil 怎么计算两个日期的天数差？"
```

### 3. 项目级定制（可选）

如果你的项目有特殊的 Hutool 使用规范，可以在 `project/` 目录下创建配置：

```bash
# 创建项目专属配置
touch project/my-awesome-project.md
```

在文件中声明你的项目约定，例如：

```markdown
---
project: my-awesome-project
hutool-version: 5.8.25
---

## 项目规范

- 禁止直接使用 `DateUtil`，统一使用项目封装的 `DateHelper`
- `BeanUtil.copyProperties` 必须指定 `ignoreNullValue = true`
- 所有 ID 生成统一使用 `IdUtil.getSnowflakeNextIdStr()`
```

### 4. 个人偏好（可选）

编辑根目录下的 `memory.md`，配置你的个人编码偏好。详见 [memory.md](memory.md)。

## 📁 目录结构

```
hutool-core-util-skill/
├── SKILL.md                # 核心：路由导航与触发词映射
├── README.md               # 项目说明、安装与使用指南（本文件）
├── memory.md               # （可选）用户个性化偏好配置
├── project/                # （可选）项目级工具类规范
│   ├── README.md
│   └── <your-project>.md
└── data/                   # 核心知识库：按功能分类的工具类文档
    ├── 01-string-util.md
    ├── 02-collection-util.md
    ├── ...
    └── 13-other-util.md
```

## 🤝 贡献指南

### 新增工具类文档

1. 在 `data/` 目录下按编号创建 `.md` 文件。
2. 遵循 `SKILL.md` 中第 6 节定义的知识库文件规范。
3. 在 `SKILL.md` 的路由表中添加对应的触发词映射。

### 文档结构规范

每个 `data/*.md` 文件应包含：

- YAML Frontmatter（title / classes / since / tags）
- 概述
- Maven 依赖
- 常用方法速查表
- 详细 API 与示例
- 常见问题 FAQ
- 最佳实践

## 📋 适用版本

| Hutool 版本 | 支持状态 |
|-------------|---------|
| 6.x | ✅ 完整支持 |
| 5.8.x | ✅ 完整支持 |
| 5.0.x ~ 5.7.x | ⚠️ 部分 API 可能不可用 |
| 4.x 及以下 | ❌ 不支持 |

## 📜 许可证

本知识库仅供 AI 编码助手使用，内容基于 [Hutool 官方文档](https://hutool.cn/docs/) 整理。
Hutool 本身遵循 [MulanPSL-2.0](https://license.coscl.org.cn/MulanPSL2) 开源许可证。
