# Project 目录说明

> 本目录用于存放**项目级**的 Hutool 使用规范与约束配置。

## 📖 用途

当你的项目对 Hutool 工具类有特定的使用规范时（如封装了统一工具类、限制了某些 API、
指定了特定版本等），可以在本目录下创建一个以项目名称命名的 Markdown 文件。

AI 助手在回答问题时，会**优先参考项目级规范**，然后再参考 `data/` 下的通用知识文档。

## 🚀 快速开始

### 1. 创建配置文件

```bash
# 以你的项目名称命名
touch project/my-awesome-project.md
```

### 2. 编写配置

使用以下模板：

```markdown
---
project: my-awesome-project
hutool-version: 5.8.25
package-style: cn.hutool
---

# [项目名称] — Hutool 使用规范

## 版本信息

- **Hutool 版本**：5.8.25
- **引入方式**：hutool-all / hutool-bom 按需引入

## 禁用 API

以下 API 在本项目中**禁止直接使用**，请使用项目封装的替代方案：

| 禁用 API | 替代方案 | 原因 |
|-----------|---------|------|
| `DateUtil` 全部方法 | `ProjectDateHelper` | 项目统一使用 Java 8 Time API |
| `BeanUtil.copyProperties` (无参) | `BeanHelper.copy` | 必须携带 CopyOptions |

## 推荐实践

- `NumberUtil.div` 必须指定 `scale` 和 `RoundingMode`
- `IdUtil` 统一使用 `getSnowflakeNextIdStr()`，禁止 `simpleUUID`
- 所有文件操作必须使用 try-with-resources 或在 finally 中关闭资源

## 项目封装工具类

| 工具类 | 位置 | 功能 |
|--------|------|------|
| `ProjectDateHelper` | `com.xxx.util.DateHelper` | 统一日期处理 |
| `BeanHelper` | `com.xxx.util.BeanHelper` | 带默认 CopyOptions 的 Bean 拷贝 |
| `ProjectIdGenerator` | `com.xxx.util.IdGenerator` | 统一 ID 生成策略 |
```

## 📝 配置项说明

### YAML Frontmatter

| 字段 | 必填 | 说明 |
|------|------|------|
| `project` | ✅ | 项目名称，与文件名一致 |
| `hutool-version` | ✅ | 项目使用的 Hutool 版本 |
| `package-style` | ❌ | 包名风格：`cn.hutool`（5.x）或 `org.dromara.hutool`（6.x） |

### 正文内容（建议包含）

- **版本信息**：Hutool 版本与引入方式
- **禁用 API**：不允许直接使用的 API 及替代方案
- **推荐实践**：项目约定的使用规范
- **项目封装工具类**：自定义工具类的清单与位置

## ⚠️ 注意事项

1. 每个项目对应一个 `.md` 文件，文件名即项目标识。
2. 项目级规范的优先级高于 `memory.md` 中的通用偏好。
3. 如果同时存在多个项目配置，AI 会根据上下文（如 `import` 语句中的包名）自动判断当前项目。
4. 配置修改后，下次对话即自动生效。
