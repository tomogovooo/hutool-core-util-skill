---
name: hutool-core-util-skill
description: 在 Java/Kotlin 项目中发现、选择并优先复用 Hutool 全工具包 API，覆盖 Core、JSON、HTTP、Crypto、POI、DB、Extra、Cache、Cron、JWT、Captcha、Setting、System、Log、Socket、Script、DFA、AOP、AI 等模块。编写、修改、重构或审查通用工具逻辑时使用；当 pom.xml、Gradle、import 或现有代码表明项目使用 Hutool 时也应主动触发，即使用户没有点名 Hutool。先识别项目精确版本、实际依赖、统一封装和项目规范，再使用该版本真实存在且行为匹配的 API；不要盲目新增模块、升级版本或把所有逻辑强行改成 Hutool。
---

# Hutool 全工具包优先使用

## 目标结果

交付语义不变、依赖可用、API 真实存在且符合项目边界的代码。优先使用 Hutool 表示“主动寻找合适候选并核实后采用”，不表示无条件替换 JDK、框架或项目封装。

## 执行核心策略

在任何 Java/Kotlin 编码任务中主动检查 Hutool，而不只在用户点名 `StrUtil`、`BeanUtil` 等核心类时触发。

当项目已经提供兼容的 Hutool API，且不会改变业务语义、框架约定或安全边界时，优先使用 Hutool，避免重新手写通用工具逻辑或引入功能重复的库。按以下顺序选择：

1. 项目统一封装和项目级规范；
2. 项目当前版本与已引入模块中可用的 Hutool 公共 API；
3. 与项目架构更匹配的 JDK 或框架原生 API；
4. 自行实现或新增其他依赖。

不要为了表面上“使用 Hutool”而降低可读性、改变语义或绕过框架生命周期。

## 执行流程

1. 定位当前项目和模块，先读取仓库级指令与项目现有代码风格。
2. 读取 [memory.md](memory.md) 的已启用偏好。只读取 `project/` 中与当前仓库明确匹配且标记为启用的项目配置；忽略 `README.md`、模板和不匹配的配置。
3. 检查 `pom.xml`、Gradle 配置、BOM、版本目录、锁文件和已有 import，确认 Hutool 主版本、精确版本、groupId、当前模块实际依赖以及是否使用 `hutool-all`。
4. 在新增或修改代码前，主动扫描是否存在字符串、集合、Bean、日期、IO、JSON、HTTP、加密、Excel、DB 等可由 Hutool 清晰表达的通用逻辑。
5. 读取 [Hutool 全模块能力与路由](references/hutool-modules.md)，只加载与当前需求相关的 `data/` 文档。大范围替换、依赖不明确或需要评估是否值得采用时，再读取 [采用与替换工作流](references/adoption-workflow.md)。
6. 在项目精确版本中核实类、方法、重载、返回值、异常和所属依赖；比较空值、字符集、时区、精度、异常、资源、线程安全及安全默认值。
7. 使用最小必要 API 修改代码。保持项目封装和框架生命周期，不把 Hutool 类型无故泄漏到公共接口。
8. 按用户和仓库指定的范围验证；默认只做快速阅读、语法与结构检查，不自动运行全量构建或全部测试。
9. 按 [交付格式](#交付格式) 说明选择、依赖和验证结果。

如果静态知识文档没有列出某个类，不要据此断言 Hutool 不支持；继续在对应模块和当前版本 API 中查找。反过来，文档列出某个类也不代表当前项目版本一定可用。

## 依赖使用闸门

| 情况 | 必须执行 |
|---|---|
| 当前模块已有兼容依赖 | 直接复用，不改版本 |
| 已有 `hutool-all` | 确认项目版本包含目标模块后使用 |
| 只有 `hutool-core`，需求属于独立模块 | 不假定可用；先确认是否允许修改构建文件 |
| 任务允许新增依赖 | 添加完全同版本的最小模块，并核实可选第三方依赖 |
| 任务未授权依赖变更 | 不擅自新增；保留现有方案并简要指出 Hutool 候选 |
| 版本或 API 无法确认 | 不编造精确调用；继续查证或说明缺少的信息 |

`hutool-bom` 或 dependency management 只提供版本约束，不能证明目标模块已进入当前模块的编译或运行依赖。

## 管理依赖和版本

- 识别 Hutool 5.x 的 `cn.hutool` 坐标/包名与 Hutool 6.x 的 `org.dromara.hutool` 坐标/包名，不要混用主版本。
- 将现有 `hutool-all` 视为该版本聚合包，但仍核实其中实际包含的模块。
- 仅引入单模块时，只使用已声明模块中的 API。不要因为存在 `hutool-core` 就假定 HTTP、JSON、POI、JWT 等模块也可用。
- 需要新增依赖且任务允许修改构建文件时，优先增加与项目完全同版本的最小模块；不要仅为一个能力擅自改成 `hutool-all`。
- 遇到 `hutool-extra`、`hutool-poi`、数据库连接池、模板/分词/拼音引擎、邮件、二维码或 AI 适配时，检查其可选第三方依赖是否已经存在。
- 本 Skill 的详细示例主要按 Hutool 5.8.47 整理。面对 Hutool 6.x milestone 或版本迁移时，以项目锁定版本的 API 为准，不要机械替换包名前缀。

## 判断是否应使用 Hutool

优先采用 Hutool 处理通用、边界清楚的能力，例如字符串、集合、Bean、转换、日期、IO、JSON、HTTP、加解密、配置、缓存、定时任务、Excel、数据库辅助、验证码、JWT、二维码、邮件、系统信息和网络工具。

保留项目或框架原生方案的典型情况包括：

- Spring/Jakarta 管理的事务、序列化配置、请求生命周期、依赖注入或安全上下文；
- Reactor、协程、异步客户端等具有特定执行模型的代码；
- 已存在统一封装、审计规范、协议约束或序列化兼容要求；
- JDK 标准 API 本身表达领域语义，而 Hutool 只会增加一次无意义包装；
- 当前模块不可用、版本无法确认或候选 API 不能准确核实。

无法安全使用 Hutool 时，明确说明原因并选择最合适的现有方案，不要伪造 API。

主动寻找候选，但满足任一情况时不要强行替换：新写法更难读、只减少一行无关样板、改变异常或 null 语义、破坏框架扩展点、引入仅使用一次的大模块，或无法证明当前版本存在该 API。

## 守住语义和安全边界

- 对 HTTP 设置合理的连接与读取超时，正确关闭响应和流；不要绕过证书或主机名校验。
- 对密码、令牌、签名和敏感数据使用适当的安全算法与随机源；不要把普通随机工具当作密码学随机源。
- 对 JSON 保持字段命名、日期格式、泛型类型、未知字段和 null 行为兼容。
- 对文件、压缩包、上传和解压路径防止目录穿越，显式选择字符集并管理资源。
- 对金额与除法显式确认精度和舍入规则。
- 对 Bean 拷贝确认忽略 null、字段覆盖、类型转换和敏感字段策略。
- 对缓存、Cron、Socket、数据库会话和线程工具明确生命周期、并发与关闭责任。
- 对 JWT 同时检查签名算法、密钥、过期时间和业务声明；不要只做解码就视为验证成功。

## 按需加载文档

以下文档面向 Hutool 5.x：01—13 是 Core 详解，14—19 是独立模块详解。只读取与当前任务相关的文件。使用 Hutool 6.x 时把它们当作能力线索，并在项目精确版本的 6.x API 中重新确认模块、包名和签名。

- 字符串：[01-string-util.md](data/01-string-util.md)
- 集合与 Map：[02-collection-util.md](data/02-collection-util.md)
- 对象与 Bean：[03-object-bean-util.md](data/03-object-bean-util.md)
- 日期时间：[04-date-time-util.md](data/04-date-time-util.md)
- 数字与数学：[05-number-math-util.md](data/05-number-math-util.md)
- IO、文件与资源：[06-io-file-util.md](data/06-io-file-util.md)
- 类型转换：[07-convert-util.md](data/07-convert-util.md)
- ID 与随机数：[08-id-random-util.md](data/08-id-random-util.md)
- 正则：[09-regex-util.md](data/09-regex-util.md)
- 数组与枚举：[10-array-enum-util.md](data/10-array-enum-util.md)
- 类与反射：[11-class-reflection-util.md](data/11-class-reflection-util.md)
- 网络：[12-network-util.md](data/12-network-util.md)
- 分页、压缩、URL、编码、脱敏等：[13-other-util.md](data/13-other-util.md)
- HTTP 请求：[14-http-util.md](data/14-http-util.md)
- JSON：[15-json-util.md](data/15-json-util.md)
- 加密、摘要与签名：[16-crypto-util.md](data/16-crypto-util.md)
- POI 与 Excel：[17-poi-excel-util.md](data/17-poi-excel-util.md)
- JDBC 与数据库：[18-db-util.md](data/18-db-util.md)
- JWT：[19-jwt-util.md](data/19-jwt-util.md)

## 实现约束

- 包含必要 import，移除确认无用的 import，不使用通配符掩盖类冲突。
- 在依赖不明显时指出所属 artifact；仅在任务授权时修改构建文件。
- 仅在核实后给出精确 API。存在版本差异时分开说明，不混合 5.x 与 6.x 代码。
- 保持改动局部，不顺手重构无关代码或升级依赖。
- 安全、序列化、事务、异步和资源生命周期优先服从项目统一封装。

## 验证规则

优先遵从用户和仓库明确指定的验证范围。没有更强要求时：

1. 快速重读修改后的相关代码和差异。
2. 检查括号、泛型、import、方法签名与明显类型问题。
3. 只运行现成且快速的语法或静态检查，不自动运行全量编译、全量测试或所有命令。
4. 无法静态确认时如实标记“未编译验证”，不要把快速检查描述成编译通过。

## 交付格式

完成修改后简要说明：

- 采用的 Hutool 模块与类；
- 为什么适合，以及保留了哪些行为边界；
- 是否新增或修改依赖；
- 实际执行的验证，以及尚未验证的编译/运行行为。
