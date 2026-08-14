---
title: 加密、摘要与签名工具
classes:
  - cn.hutool.crypto.SecureUtil
  - cn.hutool.crypto.digest.DigestUtil
  - cn.hutool.crypto.symmetric.AES
  - cn.hutool.crypto.asymmetric.RSA
module: hutool-crypto
verified: 5.8.47
tags: [Crypto, AES, RSA, 摘要, HMAC, 签名, SecureUtil]
---

# 加密、摘要与签名工具

本文档适用于 Hutool 5.8.x。密码学代码首先服从协议、合规要求和项目密钥管理规范，不能只按方法名选择算法。

## 目录

- [能力选择](#能力选择)
- [摘要](#摘要)
- [AES 对称加密](#aes-对称加密)
- [RSA 非对称加密](#rsa-非对称加密)
- [签名与 HMAC](#签名与-hmac)
- [安全边界](#安全边界)

## 能力选择

| 目标 | 入口 | 注意 |
|---|---|---|
| 文件/消息完整性摘要 | `DigestUtil` | 新设计优先 SHA-256 或更强算法 |
| 带共享密钥的消息认证 | `SecureUtil.hmacSha256` | 不等同于普通摘要 |
| 对称加解密 | `AES`、`SecureUtil.aes` | 必须明确模式、填充、IV/nonce 和认证标签 |
| 公钥加密 | `RSA`、`SecureUtil.rsa` | 仅适合短数据；大数据用混合加密 |
| 数字签名 | `SecureUtil.sign` | 固定算法并确认密钥类型 |

## 摘要

```java
import cn.hutool.crypto.digest.DigestUtil;

String textDigest = DigestUtil.sha256Hex("content");
String fileDigest = DigestUtil.sha256Hex(file);
```

摘要可用于完整性校验，但没有密钥，不能证明消息来自可信发送方。MD5 和 SHA-1 仅保留给明确要求的兼容或非对抗性校验，不用于新签名、密码存储或安全协议。

## AES 对称加密

```java
import cn.hutool.crypto.SecureUtil;
import cn.hutool.crypto.symmetric.AES;

import java.nio.charset.StandardCharsets;

byte[] key = keyProvider.loadAesKey();
AES aes = SecureUtil.aes(key);

String ciphertext = aes.encryptBase64(plaintext, StandardCharsets.UTF_8);
String restored = aes.decryptStr(ciphertext, StandardCharsets.UTF_8);
```

这段只展示 API，不代表推荐协议。`SecureUtil.aes(key)` 的默认转换是 `AES/ECB/PKCS5Padding`；ECB 不隐藏数据模式，也不提供完整性保护，不应作为新安全设计的默认方案。实际代码应使用项目批准的认证加密方案，明确保存/传输 IV 或 nonce、认证标签和版本信息，并保证 nonce 使用规则正确。

AES 密钥长度必须符合所用实现和策略。密钥从 KMS、密钥库或受控配置读取，不要硬编码、复用为 IV、打印到日志或用普通随机字符串临时生成。

## RSA 非对称加密

```java
import cn.hutool.crypto.SecureUtil;
import cn.hutool.crypto.asymmetric.KeyType;
import cn.hutool.crypto.asymmetric.RSA;

import java.nio.charset.StandardCharsets;

RSA rsa = SecureUtil.rsa(privateKeyBase64, publicKeyBase64);
String encrypted = rsa.encryptBase64("short secret", KeyType.PublicKey);
String decrypted = rsa.decryptStr(
        encrypted,
        KeyType.PrivateKey,
        StandardCharsets.UTF_8
);
```

RSA 明文长度受密钥长度和填充限制。不要把它用于直接加密大文件；使用随机数据密钥加密正文，再用公钥保护数据密钥。填充必须与协议对端完全一致，不能凭本地“能解密”判断互操作正确。

## 签名与 HMAC

`SecureUtil` 提供 `hmacSha256` 和 `sign` 等快捷入口。生成实现前必须先确认：

- 算法和参数由服务端固定，而不是信任输入携带的算法名。
- 签名覆盖的原始字节、字段顺序、字符集、换行和 Base64/Hex 格式与协议一致。
- 验签失败采用常量时间比较或库的验签 API，不自己比较敏感字节序列。
- HMAC 双方共享同一密钥；数字签名使用私钥签名、公钥验签，信任模型不同。

## 安全边界

- 密码存储使用 Argon2、bcrypt、scrypt 或 PBKDF2 等专用密码哈希，并带独立 salt 和合适成本；不要使用 MD5、SHA 或可逆 AES。
- 加密不自动提供防篡改；优先使用认证加密，或严格按既定协议组合加密与 MAC。
- 随机密钥、IV、nonce、salt 使用密码学安全随机源；不要使用 Core 的普通随机工具代替。
- 解密或验签失败不要泄漏区分性细节；日志不得包含密钥、明文和完整凭据。
- 轮换密钥时保留密钥 ID/版本，并设计旧数据迁移和验证窗口。
- 国密、FIPS、硬件密钥和证书链场景先确认 Provider 与运行环境，不能只在开发机验证。

所属模块为 `cn.hutool:hutool-crypto`。新增时与项目 Hutool 版本对齐；若协议已有成熟 SDK 或安全封装，优先复用该边界。

官方 API：[SecureUtil](https://plus.hutool.cn/apidocs/cn/hutool/crypto/SecureUtil.html)、[DigestUtil](https://plus.hutool.cn/apidocs/cn/hutool/crypto/digest/DigestUtil.html)、[AES](https://plus.hutool.cn/apidocs/cn/hutool/crypto/symmetric/AES.html)、[RSA](https://plus.hutool.cn/apidocs/cn/hutool/crypto/asymmetric/RSA.html)。
