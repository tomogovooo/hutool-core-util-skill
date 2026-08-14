# Hutool 全工具包优先复用 Skill

> 一个面向 AI 编码助手的 Java/Kotlin Skill：修改代码时主动发现 Hutool 可复用能力，在项目实际版本和依赖范围内优先采用正确 API。

调用名：`$hutool-core-util-skill`

这不是 Hutool 库本身，也不只是 API 速查表。它是一套给 AI 使用的工作规则和知识资料，解决 AI 只知道 `StrUtil`、重复手写工具逻辑、混用 Hutool 5.x/6.x、引用未安装模块或忽略安全边界等问题。

## 目录

- [使用 npx 安装](#使用-npx-安装)
- [它解决什么问题](#它解决什么问题)
- [AI 会怎样工作](#ai-会怎样工作)
- [如何使用](#如何使用)
- [覆盖范围](#覆盖范围)
- [依赖与版本原则](#依赖与版本原则)
- [验证策略](#验证策略)
- [目录结构](#目录结构)
- [个性化配置](#个性化配置)
- [使用边界](#使用边界)

## 使用 npx 安装

仓库地址：[tomogovooo/hutool-core-util-skill](https://github.com/tomogovooo/hutool-core-util-skill)

安装到当前项目：

```bash
npx skills add tomogovooo/hutool-core-util-skill
```

全局安装并指定给 Codex 使用：

```bash
npx skills add tomogovooo/hutool-core-util-skill --skill hutool-core-util-skill --agent codex --global --yes
```

查看仓库中可安装的 Skill：

```bash
npx skills add tomogovooo/hutool-core-util-skill --list
```

更新已经全局安装的 Skill：

```bash
npx skills update hutool-core-util-skill --global --yes
```

安装命令由 [`skills`](https://github.com/vercel-labs/skills) CLI 提供，不需要把本仓库另外发布成 npm 包。使用者需要先安装 Node.js，并确保 `npm` 和 `npx` 可用；仓库中的最新修改也需要先提交并推送到 GitHub，才能通过上述命令安装到。

## 它解决什么问题

没有这套 Skill 时，AI 容易出现以下情况：

- 明明项目已经使用 Hutool，却继续手写字符串、集合、日期、IO、JSON 等通用逻辑；
- 只使用 `hutool-core`，不知道 HTTP、Crypto、POI、DB、JWT、Extra 等独立模块；
- 看到 `hutool-core` 就错误生成 `HttpRequest`、`JSONUtil` 等当前依赖里不存在的类；
- 把 Hutool 5.x 的 `cn.hutool` 代码机械替换成 6.x 包名；
- 为一个小功能擅自引入 `hutool-all`、升级版本或破坏项目统一封装；
- 只追求代码变短，忽略 null、字符集、时区、精度、资源关闭、事务和安全行为。

这个 Skill 会先识别项目环境，再判断 Hutool 是否真的合适，最后才修改代码。

## AI 会怎样工作

| 阶段 | 行为 |
|---|---|
| 识别项目 | 检查 Maven/Gradle、BOM、版本目录、import 和已有封装 |
| 确认边界 | 判断 Hutool 主版本、精确版本、当前模块实际依赖和项目规范 |
| 发现候选 | 主动寻找可由 Hutool 替代的通用工具逻辑，不要求用户先说出类名 |
| 路由资料 | 只读取当前需求相关的模块说明和 `data/*.md`，避免一次加载全部文档 |
| 核实 API | 确认类全名、artifact、方法重载、返回值、异常和最低版本 |
| 比较语义 | 检查空值、编码、时区、精度、资源、线程安全和安全默认值 |
| 实施修改 | 使用最小范围改动，保留框架生命周期和项目统一封装 |
| 快速验证 | 默认检查修改后的代码、import、括号、泛型和明显签名问题 |
| 结果说明 | 报告使用的模块/类、依赖变化、验证范围和未验证行为 |

核心原则是：能安全使用 Hutool 时优先使用；不适合时不要为了“用了 Hutool”而强行替换。

## 如何使用

可以显式调用：

```text
使用 $hutool-core-util-skill 修改这段 Java 代码，优先复用项目已有的 Hutool API。
```

也可以给出更具体的约束：

```text
使用 $hutool-core-util-skill 检查这个模块的手写工具逻辑，不要新增依赖。
```

```text
用 Hutool HTTP 和 JSON 完成这个接口调用，保留现有超时、异常和序列化行为。
```

```text
这是 Hutool 6.x 项目，先核实当前版本 API，再修改代码，不要照搬 5.x 包名。
```

Skill 已允许隐式触发：处理 Java/Kotlin 通用工具逻辑，或项目已经出现 Hutool 依赖/import 时，AI 可以主动应用这些规则。

## 覆盖范围

| 范围 | 主要能力 |
|---|---|
| Core | 字符串、集合、Map、Bean、日期、数字、转换、IO、文件、反射、正则、ID、网络、URL、压缩、编码、脱敏等 |
| 常用独立模块 | JSON、HTTP、Crypto、POI/Excel、DB、Cron、Setting、Log、Socket |
| Hutool 5.x 扩展 | JWT、Cache、Captcha、System、Script、DFA、AOP、BloomFilter、AI 等 |
| Extra 生态 | 邮件、二维码、拼音、Emoji、FTP/SFTP、模板、分词、校验、系统监控等第三方适配 |

`data/01–13` 提供 Hutool 5.x Core 详细资料；`data/14–19` 提供 HTTP、JSON、Crypto、POI/Excel、DB、JWT 详细资料。其他模块通过 [全模块路由](references/hutool-modules.md) 定位，再按项目精确版本核实官方 API。

## 依赖与版本原则

- Hutool 5.x 使用 `cn.hutool` 坐标和包名；Hutool 6.x 使用 `org.dromara.hutool`，但不能只替换前缀。
- 详细示例主要按 Hutool 5.8.47 整理；在其他版本中使用前必须重新确认 API。
- `hutool-bom` 或 Maven `dependencyManagement` 只管理版本，不代表目标模块已经引入。
- 项目只有 `hutool-core` 时，不能直接使用 HTTP、JSON、POI、JWT 等独立模块。
- 需要新增依赖时，只有任务允许修改构建文件才添加完全同版本的最小模块。
- 不为一个方法擅自切换成 `hutool-all`，也不顺手升级 Hutool 主版本。
- POI、Extra 引擎、数据库连接池等场景还要确认对应的可选第三方依赖。

## 验证策略

默认采用快速验证：

1. 重读相关改动；
2. 检查语法结构、import、括号、泛型和方法签名；
3. 只运行已有且快速的语法/静态检查；
4. 不自动执行 Maven/Gradle 全量编译、全部测试或所有命令；
5. 无法静态确认时明确说明“未编译验证”。

用户或仓库明确指定验证命令时，以其要求为准。

## 目录结构

```text
hutool-core-util-skill/
├── SKILL.md                       # AI 必须遵循的核心流程和规则
├── agents/openai.yaml             # Skill 的显示名称、默认提示和隐式触发配置
├── memory.md                      # 跨项目默认偏好
├── project/README.md              # 项目级配置模板
├── references/
│   ├── hutool-modules.md          # Hutool 5.x/6.x 全模块导航
│   └── adoption-workflow.md       # 采用、替换、依赖和语义检查流程
└── data/
    ├── 01-string-util.md ... 13-other-util.md
    └── 14-http-util.md ... 19-jwt-util.md
```

## 个性化配置

- 在 [memory.md](memory.md) 中设置跨项目偏好，例如是否允许新增模块、回答详细度和默认验证强度。
- 按 [project/README.md](project/README.md) 创建项目配置，锁定版本、模块、禁用 API、替代方案和项目统一封装。
- 项目配置只有在 `active: true` 且仓库标识匹配时才会生效，避免错误套用其他项目规范。

配置优先级为：用户当前请求 → 仓库强制指令 → 匹配的项目配置 → `memory.md` → Skill 默认规则。

## 使用边界

- 本 Skill 不替代 Hutool 官方文档，也不能保证静态资料覆盖每个版本的全部 API。
- 不会用同步 Hutool HTTP 强行替换 Reactor、协程、Feign 或项目统一客户端。
- 不会用 Hutool JSON 绕过 Spring/Jackson 的统一序列化配置。
- 不会绕过 Spring/JTA 事务、安全框架、审计规范或资源生命周期。
- 密码学、JWT、文件、网络和数据库代码仍需遵循项目安全策略。
- 如果版本、依赖或语义无法确认，AI 应保留现有实现或说明缺少的信息，而不是编造 API。

详细执行规则见 [SKILL.md](SKILL.md)，Hutool 官方资料见 [Hutool 5.x API](https://plus.hutool.cn/apidocs/) 和 [Hutool 6.x API](https://plus.hutool.cn/apidocs6/)。
