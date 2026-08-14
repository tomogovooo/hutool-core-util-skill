# Memory — Hutool 使用偏好

本文件保存跨项目默认偏好。用户当前请求、仓库强制指令和匹配的项目配置具有更高优先级。AI 只应用“当前生效”代码块中的值；注释示例不生效。

## 当前生效

```yaml
prefer-hutool: true
hutool-scope: all-available-modules
dependency-policy: existing-first
prefer-module-specific-dependency: true
allow-new-hutool-dependency: when-authorized
allow-switch-to-hutool-all: false
api-verification: project-exact-version
show-imports: true
show-pitfalls: true
verbosity: standard
validation-policy: syntax-and-structure-first
run-full-build-by-default: false
run-full-test-suite-by-default: false
```

这些默认值表示：

- 在当前项目实际版本和依赖范围内，语义合适时优先使用 Hutool 全模块 API。
- 优先复用已有依赖；只有任务允许修改构建文件时才增加完全同版本的最小模块。
- 不为单个能力改成 `hutool-all`，不顺手升级 Hutool。
- 精确 API 必须按项目版本核实，不能只依据本 Skill 的 5.8.47 示例。
- 修改后默认快速重读并检查语法/结构，不自动执行全量编译或全部测试。

## 可选覆盖项

按需把需要的字段加入“当前生效”代码块：

```yaml
# language: java                 # java / kotlin
# hutool-version: 5.8.47        # 通常留空并从项目检测
# package-style: cn.hutool      # cn.hutool / org.dromara.hutool
# hutool-modules:               # 当前模块实际依赖，不含仅由 BOM 管理的模块
#   - hutool-core
#   - hutool-json
# code-style: traditional       # traditional / chained / functional
# show-comments: true
# show-jdk-alternative: true
# show-third-party-alternative: false
# show-since-version: true
# disabled-apis:
#   - DateUtil
#   - BeanUtil.copyProperties
# alternatives:
#   DateUtil.parse: ProjectDateHelper.parse
# conventions:
#   - BeanUtil.copyProperties 必须显式设置 CopyOptions
#   - NumberUtil.div 必须指定 scale 和 RoundingMode
```

## 字段规则

| 字段 | 可选值或含义 |
|---|---|
| `dependency-policy` | `existing-only` / `existing-first` / `allow-minimal-module` |
| `allow-new-hutool-dependency` | `never` / `when-authorized` / `always` |
| `api-verification` | `project-exact-version` / `local-api` / `official-api` |
| `verbosity` | `concise` / `standard` / `detailed` |
| `validation-policy` | `syntax-and-structure-first` / `targeted-checks` / `project-default` |

`always` 仍不能越过用户授权或仓库依赖规则；它只在任务本身允许依赖变更时表达偏好。

## 配置优先级

从高到低应用：

1. 用户当前请求；
2. 当前仓库的强制指令和统一封装；
3. `project/` 中明确匹配且 `active: true` 的项目配置；
4. 本文件“当前生效”配置；
5. Skill 默认规则。

配置中的版本、包名和模块若与构建文件冲突，以项目真实依赖为准并报告差异。
