# Hutool 全模块能力与路由

用本文档把需求路由到 Hutool 模块和详细资料。只读取与当前需求相关的表格行和链接；大范围替换或依赖决策再读取 [采用与替换工作流](adoption-workflow.md)。

## 目录

- [先确定版本边界](#先确定版本边界)
- [快速识别实际依赖](#快速识别实际依赖)
- [Hutool 5.x 模块路由](#hutool-5x-模块路由)
- [Hutool 6.x 模块路由](#hutool-6x-模块路由)
- [按需求交叉路由](#按需求交叉路由)
- [核实 API 的顺序](#核实-api-的顺序)
- [官方来源](#官方来源)

## 先确定版本边界

| 系列 | Maven groupId | Java 包名前缀 | 使用原则 |
|---|---|---|---|
| Hutool 5.x | `cn.hutool` | `cn.hutool` | 使用项目锁定的 5.x 版本和对应模块；`data/` 详解按 5.8.47 整理 |
| Hutool 6.x | `org.dromara.hutool` | `org.dromara.hutool` | 官方 API 仍可能随 milestone 演进；按项目精确版本的 POM、JAR 和 API 核实 |

不要只根据类名迁移主版本。先检查构建文件、依赖树或本地 JAR，再确认 API。

## 快速识别实际依赖

### Maven

- 读取当前模块 `<dependencies>`，再向上检查父 POM、属性和 `dependencyManagement`。
- `cn.hutool:hutool-bom` 或 `org.dromara.hutool:hutool-bom` 只管理版本；目标 artifact 仍需出现在依赖中。
- 检查 `scope`、`optional`、exclusions 和当前子模块是否真正继承。

### Gradle

- 读取当前模块的 `dependencies`、version catalog、platform 和 convention plugin。
- 区分 `implementation`、`api`、`compileOnly`、`runtimeOnly` 与测试配置。
- 锁文件或已解析依赖比只看版本目录更能证明当前模块实际可用。

### 源码信号

- `cn.hutool.*` 只能证明代码按 5.x 包名编写，`org.dromara.hutool.*` 只能证明按 6.x 包名编写。
- import 不能单独证明依赖解析成功；构建文件也不能单独证明 API 签名正确。
- 项目自有 `HutoolHelper`、HTTP 客户端、JSON 配置或事务封装优先于直接调用底层 API。

## Hutool 5.x 模块路由

以下模块来自 Hutool 5.x 官方模块清单。入口类只是检索线索，必须在项目实际版本中确认签名。

| 模块 | 适用意图 | 常见入口或检索词 |
|---|---|---|
| `hutool-core` | 字符串、集合、Bean、日期、转换、IO、文件、压缩、线程、反射、树、XML、网络、ID | [Core 详解索引](../SKILL.md#按需加载文档)、`StrUtil`, `CollUtil`, `BeanUtil`, `DateUtil`, `Convert`, `FileUtil`, `ThreadUtil`, `TreeUtil` |
| `hutool-aop` | 非 IOC 环境的动态代理、切面 | 代理、切面、`ProxyUtil`, `Aspect` |
| `hutool-bloomFilter` | 布隆过滤、快速判断可能存在性 | bloom、去重、`BloomFilter`, `BitMapBloomFilter` |
| `hutool-cache` | 本地缓存、FIFO/LFU/LRU/定时缓存 | `CacheUtil`, `LRUCache`, `TimedCache` |
| `hutool-cron` | Cron 表达式、计划任务、调度监听 | `CronUtil`, `Scheduler`, `CronPattern` |
| `hutool-crypto` | 摘要、HMAC、对称/非对称加密、国密、签名、密钥 | [Crypto 详解](../data/16-crypto-util.md)、`SecureUtil`, `DigestUtil`, `AES`, `RSA`, `SmUtil` |
| `hutool-db` | JDBC、SQL 执行、Entity、分页、事务/Session、数据源辅助 | [DB 详解](../data/18-db-util.md)、`Db`, `Entity`, `SqlRunner`, `Session`, `DSFactory` |
| `hutool-dfa` | 敏感词、多关键词匹配 | `WordTree`, `FoundWord` |
| `hutool-extra` | 邮件、二维码、拼音、Emoji、FTP/SFTP、模板、分词、校验、Servlet、XML/JAXB、系统监控等第三方适配 | `MailUtil`, `QrCodeUtil`, `PinyinUtil`, `EmojiUtil`, `Ftp`, `TemplateUtil` |
| `hutool-http` | HTTP/HTTPS 请求、表单、上传、下载、Cookie、代理 | [HTTP 详解](../data/14-http-util.md)、`HttpUtil`, `HttpRequest`, `HttpResponse`, `HttpDownloader` |
| `hutool-log` | 日志门面和日志实现自动识别 | `Log`, `LogFactory`, `StaticLog` |
| `hutool-script` | JSR-223 脚本引擎封装 | `ScriptUtil`, JavaScript、脚本执行 |
| `hutool-setting` | Setting、分组配置、Properties | `Setting`, `Props`, `GroupedMap` |
| `hutool-system` | JVM、OS、主机、用户、运行时信息 | `SystemUtil`, `JavaInfo`, `OsInfo`, `RuntimeInfo` |
| `hutool-json` | JSON 对象/数组、序列化、反序列化、JSONPath/配置 | [JSON 详解](../data/15-json-util.md)、`JSONUtil`, `JSONObject`, `JSONArray`, `JSONConfig` |
| `hutool-captcha` | 图片/GIF 验证码 | `CaptchaUtil`, `LineCaptcha`, `CircleCaptcha`, `ShearCaptcha` |
| `hutool-poi` | Excel 读取写入、大数据量 Excel、Word 辅助 | [POI/Excel 详解](../data/17-poi-excel-util.md)、`ExcelUtil`, `ExcelReader`, `ExcelWriter`, `BigExcelWriter`, `Word07Writer` |
| `hutool-socket` | NIO/AIO Socket 客户端与服务端 | `AioClient`, `AioServer`, NIO、Socket |
| `hutool-jwt` | JWT 创建、解析、签名和验证 | [JWT 详解](../data/19-jwt-util.md)、`JWT`, `JWTUtil`, `JWTValidator`, `JWTSignerUtil` |
| `hutool-ai` | AI 模型或服务适配 | AI、模型、provider；按项目版本与具体适配器核实 |
| `hutool-all` | 聚合引入该版本全部模块 | 只将它当依赖聚合，不当成功能模块 |
| `hutool-bom` | 统一管理 Hutool 模块版本 | 只负责版本约束，不能据此判断某个功能模块已经进入运行时依赖 |

## Hutool 6.x 模块路由

Hutool 6.x 的模块边界、包结构和 API 与 5.x 不完全相同。本资料核对时官方 API 标题为 `6.0.0-M20`；这不是项目版本推断依据。项目可能锁定更早或更新的 milestone，必须以实际依赖为准。常见模块包括：

- `hutool-core`
- `hutool-cron`
- `hutool-crypto`
- `hutool-db`
- `hutool-extra`
- `hutool-http`
- `hutool-log`
- `hutool-setting`
- `hutool-json`
- `hutool-poi`
- `hutool-socket`
- `hutool-swing`
- 聚合与版本管理：`hutool-all`, `hutool-bom`

不要假定 5.x 的 AOP、Cache、Captcha、JWT、System、Script、DFA、BloomFilter 或 AI 模块在 6.x 中仍以相同 artifact、包和 API 存在。部分能力可能被移动或并入其他模块；逐项搜索项目所用 6.x 版本的 POM、JAR 和官方 API。例如 `6.0.0-M20` 的 `StrUtil` 位于 `org.dromara.hutool.core.text`，JWT 类位于 `org.dromara.hutool.json.jwt`，都不能由 5.x 包名机械替换得到。

## 按需求交叉路由

| 用户需求或代码意图 | 优先检查 |
|---|---|
| JSON 转换、读取配置 JSON、Bean 与 JSON 互转 | [JSON 详解](../data/15-json-util.md)；确认项目是否统一使用 Jackson/Gson 配置 |
| 调第三方接口、上传下载文件 | [HTTP 详解](../data/14-http-util.md)；检查超时、关闭响应、TLS、代理和重试要求 |
| AES/RSA/摘要/签名/国密 | [Crypto 详解](../data/16-crypto-util.md)；先确认算法、模式、填充、编码、密钥管理 |
| Excel 导入导出、流式写入 | [POI/Excel 详解](../data/17-poi-excel-util.md)；检查 POI 版本、数据量、模板和资源关闭 |
| JDBC 小型查询、元数据、数据源 | [DB 详解](../data/18-db-util.md)；有 ORM/框架事务时优先服从框架边界 |
| 定时任务 | `hutool-cron`；检查调度器启动、停止、线程和重复注册 |
| 邮件、二维码、拼音、模板、FTP、分词 | `hutool-extra`；检查具体引擎的可选依赖 |
| 本地缓存 | 5.x `hutool-cache`；分布式或 Spring Cache 场景不要错误替代 |
| JWT | [JWT 详解](../data/19-jwt-util.md)；5.x `hutool-jwt` 必须验证签名、算法、过期时间与业务声明 |
| 验证码 | 5.x `hutool-captcha`；检查服务端存储、时效和防重放策略 |
| 敏感词、多关键词搜索 | 5.x `hutool-dfa`；普通单个正则继续考虑 Core `ReUtil` |
| 系统/JVM 信息 | 5.x `hutool-system` 或相应版本的 Extra management 适配 |
| 日志 | `hutool-log`；已有 SLF4J 规范时不要破坏占位符和异常记录方式 |
| Socket | `hutool-socket`；明确协议、半包、线程、超时与关闭责任 |

## 核实 API 的顺序

1. 查当前代码库的已有用法和统一封装。
2. 查当前模块构建文件中的精确版本、artifact 和依赖作用域。
3. 查 IDE/本地依赖缓存中的源码、JAR 或 Javadoc。
4. 查完全匹配版本的 Hutool 官方 API 或源码分支。
5. 同时确认可选第三方依赖、废弃状态和最低版本。
6. 只有确认类和签名真实存在后才生成代码。

如果无法核实，给出模块级建议或继续使用已知正确的现有实现，并说明缺少哪项版本信息。不要凭相似方法名补全 API。

## 官方来源

- Hutool 5.x 源码与模块清单（`v5-master`）：<https://github.com/dromara/hutool/tree/v5-master>
- Hutool 5.8.47 API：<https://plus.hutool.cn/apidocs/>
- Hutool 6.x 源码与模块：<https://github.com/dromara/hutool>
- Hutool 6.x API 入口：<https://plus.hutool.cn/apidocs6/>
