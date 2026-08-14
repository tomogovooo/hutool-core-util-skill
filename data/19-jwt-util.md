---
title: JWT 创建与验证工具
classes:
  - cn.hutool.jwt.JWT
  - cn.hutool.jwt.JWTUtil
  - cn.hutool.jwt.JWTValidator
  - cn.hutool.jwt.signers.JWTSignerUtil
module: hutool-jwt
verified: 5.8.47
tags: [JWT, JWS, Token, 签名, 验证, JWTUtil]
---

# JWT 创建与验证工具

本文档明确面向 Hutool 5.8.x；`JWT` 自 5.7.0 提供。Hutool 6.x 当前模块布局与 5.x 不同，生成 v6 代码前必须确认项目精确版本是否存在对应模块、坐标和 API。

## 目录

- [入口选择](#入口选择)
- [创建令牌](#创建令牌)
- [解析并完整验证](#解析并完整验证)
- [JWTUtil 快捷方法](#jwtutil-快捷方法)
- [业务声明校验](#业务声明校验)
- [安全边界](#安全边界)

## 入口选择

| 场景 | API |
|---|---|
| 链式设置注册声明并签名 | `JWT.create()` |
| 解析已有 Token | `JWT.of(token)` |
| 简单创建/解析/验签 | `JWTUtil` |
| 分开校验算法和时间声明 | `JWTValidator` |
| 明确创建 HS/RS/ES 签名器 | `JWTSignerUtil` |

解析只会读出 Header 和 Payload，不等于令牌可信。读取用户身份或权限前必须完成签名、时间和业务声明校验。

## 创建令牌

```java
import cn.hutool.core.date.DateUtil;
import cn.hutool.jwt.JWT;

import java.util.Date;

Date now = new Date();
String token = JWT.create()
        .setIssuer("auth.example.com")
        .setSubject(userId)
        .setAudience("orders-api")
        .setIssuedAt(now)
        .setNotBefore(now)
        .setExpiresAt(DateUtil.offsetMinute(now, 15))
        .setJWTId(tokenId)
        .setPayload("role", role)
        .setSigner("HS256", signingKey)
        .sign();
```

`signingKey` 应来自密钥管理系统或受控配置。HS256 使用共享密钥；跨服务或第三方验证场景可按系统设计使用非对称签名器，让验证方只持有公钥。

## 解析并完整验证

```java
import cn.hutool.json.JSONArray;
import cn.hutool.jwt.JWT;

JWT jwt = JWT.of(token).setSigner("HS256", signingKey);
boolean valid = jwt.validate(30);
if (!valid) {
    throw new SecurityException("Invalid token");
}

Object audienceClaim = jwt.getPayload("aud");
boolean expectedAudience = audienceClaim instanceof JSONArray
        ? ((JSONArray) audienceClaim).contains("orders-api")
        : "orders-api".equals(audienceClaim);

if (!"auth.example.com".equals(jwt.getPayload("iss")) || !expectedAudience) {
    throw new SecurityException("Unexpected token claims");
}

String subject = String.valueOf(jwt.getPayload("sub"));
```

`validate(leewaySeconds)` 会验证签名以及 `nbf`、`exp`、`iat` 时间声明。允许的时钟偏差应尽量小并来自配置；它不是延长令牌寿命的机制。

Hutool 的 `setAudience(String...)` 把受众作为数组写入；外部 JWT 按标准也可能把单个受众写成字符串。验证 `aud` 时兼容这两种形态并检查“是否包含当前服务”，不要直接把数组对象和字符串比较。

## JWTUtil 快捷方法

```java
import cn.hutool.jwt.JWT;
import cn.hutool.jwt.JWTUtil;

String token = JWTUtil.createToken(payloads, signingKey);

if (!JWTUtil.verify(token, signingKey)) {
    throw new SecurityException("Bad signature");
}

JWT parsed = JWTUtil.parseToken(token);
```

Hutool 5.x 的 `createToken(Map, byte[])` 默认使用 HS256。`JWTUtil.verify` 用于验签；涉及有效期时继续使用 `JWT.validate` 或 `JWTValidator.validateDate`，不能只验签后永久接受 Token。

## 业务声明校验

签名和时间有效仍不代表令牌适用于当前请求。至少按协议检查：

- `iss`：是否来自允许的签发方。
- `aud`：是否包含当前服务，避免一个服务的 Token 被另一个服务接受。
- `sub`：主体格式是否有效且账号仍可用。
- `jti`：注销、一次性 Token 或高风险操作是否需要防重放。
- 权限、租户、Token 类型：是否在服务器允许范围内，不能信任未知自定义声明。

## 安全边界

- JWT Payload 只是 Base64URL 编码的明文，不存放密码、私钥或无需暴露的个人数据。
- 固定允许的算法并使用对应密钥。不要仅按 Token Header 的 `alg` 自动选择算法，避免算法混淆。
- 禁止接受 `none`；区分 HMAC 密钥与 RSA/EC 公私钥，不让同一字节材料跨算法复用。
- 密钥必须足够强，支持轮换并通过受控 `kid` 映射到服务端白名单；未知 `kid` 立即拒绝。
- 令牌应短时有效。刷新、撤销、注销、密码修改和权限变更需要额外服务端策略。
- 不在 URL 查询参数和日志中记录完整 Token；优先从受保护的 Authorization Header 传输。
- 验证失败对外返回统一错误，不暴露验签、过期或声明失败的内部细节。

所属模块为 `cn.hutool:hutool-jwt`，Hutool 5.x 中它依赖 JSON 与 Crypto 能力。不要只因已有 `hutool-core` 就生成 JWT 代码。

官方 API：[JWT](https://plus.hutool.cn/apidocs/cn/hutool/jwt/JWT.html)、[JWTUtil](https://plus.hutool.cn/apidocs/cn/hutool/jwt/JWTUtil.html)、[JWTValidator](https://plus.hutool.cn/apidocs/cn/hutool/jwt/JWTValidator.html)。
