# Project 配置模板

本文件只是说明和模板，不是有效项目配置。Skill 必须忽略 `README.md`、以下划线开头的文件，以及 `active` 不是 `true` 的配置。

## 目录

- [用途](#用途)
- [创建有效配置](#创建有效配置)
- [配置项说明](#配置项说明)
- [匹配和优先级](#匹配和优先级)

## 用途

当你的项目对 Hutool 工具类有特定的使用规范时（如封装了统一工具类、限制了某些 API、
指定了特定版本等），可以在本目录下创建一个以项目名称命名的 Markdown 文件。

项目配置用于覆盖 `memory.md` 的跨项目默认偏好，例如锁定版本、限制模块、禁止 API、指定项目封装和验证范围。

## 创建有效配置

在本目录创建与项目对应的 Markdown 文件，例如 `my-awesome-project.md`。不要复制为 `README.md`。

使用以下模板，并把 `active` 设为 `true`：

```markdown
---
project: my-awesome-project
active: true
match-root-names:
  - my-awesome-project
match-artifact-ids:
  - my-awesome-service
hutool-version: 5.8.47
package-style: cn.hutool
hutool-modules:
  - hutool-core
  - hutool-json
  - hutool-http
dependency-policy: existing-first
allow-new-hutool-dependency: when-authorized
validation-policy: syntax-and-structure-first
---

# [项目名称] — Hutool 使用规范

## 版本信息

- **Hutool 版本来源**：父 POM 的 `hutool.version`
- **引入方式**：按模块引入，BOM 只负责版本约束

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

## 依赖规则

- 允许复用当前模块已声明的 Hutool 模块。
- 新增模块前必须确认任务允许修改构建文件。
- 禁止为了单个方法切换为 `hutool-all` 或升级 Hutool 主版本。

## 验证规则

- 默认快速检查修改后的代码、import、括号、泛型和方法签名。
- 不自动执行全量编译或全部测试；用户明确要求时再执行指定命令。

## 项目封装工具类

| 工具类 | 位置 | 功能 |
|--------|------|------|
| `ProjectDateHelper` | `com.xxx.util.DateHelper` | 统一日期处理 |
| `BeanHelper` | `com.xxx.util.BeanHelper` | 带默认 CopyOptions 的 Bean 拷贝 |
| `ProjectIdGenerator` | `com.xxx.util.IdGenerator` | 统一 ID 生成策略 |
```

## 配置项说明

### YAML Frontmatter

| 字段 | 必填 | 说明 |
|------|------|------|
| `project` | ✅ | 项目名称，与文件名一致 |
| `active` | ✅ | 只有 `true` 才是有效配置 |
| `match-root-names` / `match-artifact-ids` | ✅ | 用于证明当前仓库与配置匹配 |
| `hutool-version` | ✅ | 项目实际使用的精确版本 |
| `package-style` | ❌ | 包名风格：`cn.hutool`（5.x）或 `org.dromara.hutool`（6.x） |
| `hutool-modules` | ✅ | 当前模块实际引入的模块；BOM 不算已引入 |
| `dependency-policy` | ❌ | `existing-only`、`existing-first` 或项目自定义依赖策略 |
| `validation-policy` | ❌ | 默认验证强度；不能覆盖用户当前请求 |

### 正文内容（建议包含）

- **版本信息**：Hutool 版本与引入方式
- **禁用 API**：不允许直接使用的 API 及替代方案
- **推荐实践**：项目约定的使用规范
- **项目封装工具类**：自定义工具类的清单与位置

## 匹配和优先级

1. 必须同时满足 `active: true` 和至少一个仓库标识匹配，不能只凭相似名称套用配置。
2. 多个配置同时匹配时，优先使用根目录匹配更具体的配置；仍冲突则停止套用冲突字段。
3. 项目配置高于 `memory.md`，但低于用户当前请求和仓库强制指令。
4. 配置写了版本或模块仍要与构建文件核对；不一致时以项目真实依赖为准并报告差异。
