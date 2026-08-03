# Memory — 用户个性化偏好配置

> 本文件记录你对 Hutool 工具类使用的个性化偏好。
> AI 助手在生成回答时会读取并遵从这些配置。
> 按需修改下方各项，删除 `#` 注释符号即可启用对应配置。

---

## 基础偏好

```yaml
# 偏好的编程语言（java / kotlin）
# language: java

# Hutool 版本锁定（如不指定，默认同时覆盖 5.x 和 6.x）
# hutool-version: 5.8.25

# 包名风格（hutool5 使用 cn.hutool，hutool6 使用 org.dromara.hutool）
# package-style: cn.hutool
```

## 代码风格

```yaml
# 偏好的代码风格
#   - traditional   传统写法，一步一步赋值
#   - chained       链式调用风格
#   - functional    函数式 / Stream 风格
# code-style: traditional

# 是否在示例中包含完整 import 语句
# show-imports: true

# 是否在示例中包含注释说明
# show-comments: true

# 缩进风格（spaces / tabs）
# indent-style: spaces

# 缩进宽度
# indent-width: 4
```

## 回答偏好

```yaml
# 是否显示 JDK 原生 API 的对比方案
# show-jdk-alternative: true

# 是否显示 Apache Commons / Guava 的对比方案
# show-third-party-alternative: false

# 回答详细程度
#   - concise    简洁模式：只给核心代码和一句话说明
#   - standard   标准模式：代码 + 参数说明 + 注意事项
#   - detailed   详细模式：代码 + 参数说明 + 注意事项 + 原理解析 + 对比
# verbosity: standard

# 是否标注方法的最低版本要求
# show-since-version: true

# 是否显示常见陷阱与注意事项
# show-pitfalls: true
```

## 项目约束

```yaml
# 禁用的工具类或方法（逗号分隔）
# 示例：项目中已封装了自己的日期工具类，禁止直接使用 DateUtil
# disabled-apis: DateUtil, BeanUtil.copyProperties

# 推荐的替代方案映射
# 格式：原始API -> 替代方案
# alternatives:
#   - DateUtil.parse -> ProjectDateHelper.parse
#   - IdUtil.simpleUUID -> ProjectIdGenerator.generate

# 必须遵循的编码规约
# conventions:
#   - BeanUtil.copyProperties 必须设置 ignoreNullValue=true
#   - NumberUtil.div 必须指定精度和舍入模式
#   - FileUtil 操作必须使用 try-with-resources
```

---

## 使用说明

1. **启用配置**：将你需要的配置项前面的 `#` 删除即可生效。
2. **多值配置**：部分配置项支持列表格式，使用 YAML 列表语法。
3. **优先级**：`project/<项目名>.md` 中的配置会覆盖本文件中的同名配置。
4. **实时生效**：修改保存后，下次对话即自动应用新配置。

### 配置示例

一个 Kotlin 开发者的典型配置：

```yaml
language: kotlin
hutool-version: 5.8.25
package-style: cn.hutool
code-style: chained
show-imports: true
show-jdk-alternative: false
show-third-party-alternative: false
verbosity: concise
show-pitfalls: true
```

一个注重代码质量的 Java 团队配置：

```yaml
language: java
hutool-version: 6.0.0
package-style: org.dromara.hutool
code-style: traditional
show-imports: true
show-jdk-alternative: true
show-third-party-alternative: true
verbosity: detailed
show-since-version: true
show-pitfalls: true
disabled-apis: DateUtil
conventions:
  - BeanUtil.copyProperties 必须设置 CopyOptions
  - NumberUtil.div 必须指定 scale 和 RoundingMode
```
