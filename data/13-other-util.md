---
title: 其他常用工具
classes:
  - cn.hutool.core.util.PageUtil
  - cn.hutool.core.util.ZipUtil
  - cn.hutool.core.util.URLUtil
  - cn.hutool.core.util.HexUtil
  - cn.hutool.core.util.HashUtil
  - cn.hutool.core.util.EscapeUtil
  - cn.hutool.core.util.CharUtil
  - cn.hutool.core.util.BooleanUtil
  - cn.hutool.core.util.DesensitizedUtil
since: 3.0.0
tags: [分页, 压缩, URL, Hex, Hash, 转义, 脱敏, PageUtil, ZipUtil, DesensitizedUtil]
---

# 其他常用工具

本文档涵盖 `hutool-core` 中零散但高频使用的工具类。

---

## PageUtil — 分页工具

### 概述

分页计算工具，提供页码与偏移量互转、总页数计算、彩虹分页等功能。

### 核心方法

| 方法名 | 功能 | 返回类型 |
|--------|------|----------|
| `setFirstPageNo(pageNo)` | 设置起始页码（0 或 1） | `void` |
| `getStart(pageNo, pageSize)` | 计算分页起始位置 | `int` |
| `transToStartEnd(pageNo, pageSize)` | 转为 [start, end] 数组 | `int[]` |
| `totalPage(totalCount, pageSize)` | 计算总页数 | `int` |
| `rainbow(currentPage, totalPage, displayCount)` | 彩虹分页 | `int[]` |

### 示例

```java
import cn.hutool.core.util.PageUtil;

// 设置页码从1开始（默认从0开始）
PageUtil.setFirstPageNo(1);

// 计算起始位置：第2页，每页10条
int start = PageUtil.getStart(2, 10);  // => 10

// 转为 [start, end]
int[] range = PageUtil.transToStartEnd(2, 10);  // => [10, 20]

// 计算总页数
int total = PageUtil.totalPage(55, 10);  // => 6

// 彩虹分页（当前第5页，共20页，显示5个页码）
int[] pages = PageUtil.rainbow(5, 20, 5);  // => [3, 4, 5, 6, 7]
```

> ⚠️ **注意**：`setFirstPageNo` 是全局设置，影响所有后续调用。建议在应用启动时设置一次。

---

## ZipUtil — 压缩工具

### 概述

文件/目录的压缩与解压工具，支持 ZIP、GZIP、ZLIB 格式。

### 核心方法

| 方法名 | 功能 | 返回类型 |
|--------|------|----------|
| `zip(srcPath)` | 压缩文件/目录为同名 .zip | `File` |
| `zip(srcPath, zipPath)` | 压缩到指定路径 | `File` |
| `zip(zipPath, paths...)` | 多个文件压缩到一个 zip | `File` |
| `unzip(zipPath)` | 解压到同级目录 | `File` |
| `unzip(zipPath, destPath)` | 解压到指定目录 | `File` |
| `unzip(zipPath, destPath, charset)` | 指定编码解压 | `File` |
| `gzip(data)` | GZIP 压缩字节数组 | `byte[]` |
| `unGzip(data)` | GZIP 解压字节数组 | `byte[]` |
| `zlib(data, level)` | ZLIB 压缩 | `byte[]` |
| `unZlib(data)` | ZLIB 解压 | `byte[]` |

### 示例

```java
import cn.hutool.core.util.ZipUtil;

// 压缩目录
ZipUtil.zip("d:/mydir");
// => 生成 d:/mydir.zip

// 压缩到指定路径
ZipUtil.zip("d:/mydir", "d:/backup/archive.zip");

// 解压
ZipUtil.unzip("d:/archive.zip", "d:/output");

// 中文文件名乱码时指定编码
ZipUtil.unzip("d:/中文.zip", "d:/output", CharsetUtil.CHARSET_GBK);

// GZIP 压缩/解压
byte[] compressed = ZipUtil.gzip("Hello World".getBytes());
byte[] decompressed = ZipUtil.unGzip(compressed);
```

> ⚠️ **中文乱码**：Windows 下创建的 ZIP 可能使用 GBK 编码，解压时需指定 `CharsetUtil.CHARSET_GBK`。

---

## URLUtil — URL 工具

### 概述

URL 的创建、编解码、路径提取等操作。

### 核心方法

| 方法名 | 功能 | 返回类型 |
|--------|------|----------|
| `url(urlStr)` | 字符串转 URL | `URL` |
| `normalize(url)` | URL 标准化 | `String` |
| `encode(url)` | URL 编码 | `String` |
| `decode(url)` | URL 解码 | `String` |
| `getPath(url)` | 获取 URL 路径 | `String` |
| `getHost(url)` | 获取主机名 | `String` |
| `toURI(url)` | URL 转 URI | `URI` |

### 示例

```java
import cn.hutool.core.util.URLUtil;

// 标准化 URL（自动补全协议、清理路径）
URLUtil.normalize("hutool.cn");
// => "http://hutool.cn"

URLUtil.normalize("http://hutool.cn//docs/../api");
// => "http://hutool.cn/api"

// URL 编解码
String encoded = URLUtil.encode("你好 世界");
// => "%E4%BD%A0%E5%A5%BD+%E4%B8%96%E7%95%8C"

String decoded = URLUtil.decode(encoded);
// => "你好 世界"

// 提取路径
String path = URLUtil.getPath("https://hutool.cn/docs/guide/start");
// => "/docs/guide/start"
```

---

## HexUtil — 十六进制工具

### 概述

字节数组与十六进制字符串的互转工具。

### 核心方法

| 方法名 | 功能 | 返回类型 |
|--------|------|----------|
| `encodeHexStr(data)` | 字节数组转十六进制字符串 | `String` |
| `encodeHexStr(str, charset)` | 字符串转十六进制 | `String` |
| `decodeHexStr(hexStr)` | 十六进制转字符串 | `String` |
| `decodeHex(hexStr)` | 十六进制转字节数组 | `byte[]` |
| `toHex(value)` | 数字转十六进制 | `String` |
| `hexToInt(hex)` | 十六进制转 int | `int` |
| `isHexNumber(str)` | 是否十六进制数 | `boolean` |

### 示例

```java
import cn.hutool.core.util.HexUtil;

// 字符串 ↔ 十六进制
String hex = HexUtil.encodeHexStr("Hello");      // "48656c6c6f"
String str = HexUtil.decodeHexStr("48656c6c6f");  // "Hello"

// 数字 ↔ 十六进制
String numHex = HexUtil.toHex(255);   // "ff"
int num = HexUtil.hexToInt("ff");      // 255

// 判断
HexUtil.isHexNumber("0xFF");  // true
HexUtil.isHexNumber("abc");   // false
```

---

## HashUtil — 哈希工具

### 概述

提供多种非加密哈希算法的实现，适用于哈希表、布隆过滤器、一致性哈希等场景。

### 核心方法

| 方法名 | 功能 |
|--------|------|
| `cityHash32(data)` | CityHash 32位 |
| `cityHash64(data)` | CityHash 64位 |
| `cityHash128(data)` | CityHash 128位 |
| `murmur32(data)` | MurmurHash 32位 |
| `murmur64(data)` | MurmurHash 64位 |
| `murmur128(data)` | MurmurHash 128位 |
| `fnvHash(str)` | FNV Hash |
| `additiveHash(str, prime)` | 加法哈希 |
| `rotatingHash(str, prime)` | 旋转哈希 |

### 示例

```java
import cn.hutool.core.util.HashUtil;

// MurmurHash（推荐，分布均匀性好）
int hash32 = HashUtil.murmur32("Hello World".getBytes());
long hash64 = HashUtil.murmur64("Hello World".getBytes());

// FNV Hash
int fnv = HashUtil.fnvHash("Hello");

// 用于一致性哈希、布隆过滤器等
int hash = HashUtil.murmur32(key.getBytes());
int bucket = Math.abs(hash) % bucketCount;
```

---

## EscapeUtil — 转义工具

### 概述

HTML/JavaScript 特殊字符的转义与反转义。

### 核心方法

| 方法名 | 功能 |
|--------|------|
| `escape(str)` | 转义特殊字符 |
| `unescape(str)` | 反转义 |
| `safeUnescape(str)` | 安全反转义 |

### 示例

```java
import cn.hutool.core.util.EscapeUtil;

// 转义
String escaped = EscapeUtil.escape("Hello <World> & \"Hutool\"");
// => "Hello%20%3CWorld%3E%20%26%20%22Hutool%22"

// 反转义
String unescaped = EscapeUtil.unescape(escaped);
// => "Hello <World> & \"Hutool\""
```

---

## CharUtil — 字符工具

### 概述

单个字符的类型判断与操作。

### 核心方法

| 方法名 | 功能 | 返回类型 |
|--------|------|----------|
| `isAscii(ch)` | 是否 ASCII | `boolean` |
| `isLetter(ch)` | 是否字母 | `boolean` |
| `isLetterUpper(ch)` | 是否大写字母 | `boolean` |
| `isLetterLower(ch)` | 是否小写字母 | `boolean` |
| `isNumber(ch)` | 是否数字 | `boolean` |
| `isBlankChar(ch)` | 是否空白字符 | `boolean` |
| `isFileSeparator(ch)` | 是否文件路径分隔符 | `boolean` |
| `equals(a, b, ignoreCase)` | 字符比较 | `boolean` |

### 示例

```java
import cn.hutool.core.util.CharUtil;

CharUtil.isAscii('A');           // true
CharUtil.isLetter('中');         // true
CharUtil.isLetterUpper('A');     // true
CharUtil.isNumber('5');          // true
CharUtil.isBlankChar(' ');       // true
CharUtil.isBlankChar('\t');      // true
CharUtil.isFileSeparator('/');   // true
CharUtil.isFileSeparator('\\');  // true
```

---

## BooleanUtil — 布尔工具

### 概述

布尔值与其他类型的互转、逻辑运算。

### 核心方法

| 方法名 | 功能 | 返回类型 |
|--------|------|----------|
| `toBoolean(str)` | 字符串转布尔 | `boolean` |
| `toInt(boolVal)` | 布尔转 int（true=1, false=0） | `int` |
| `toStringTrueFalse(bool)` | 转 "true"/"false" | `String` |
| `toStringYesNo(bool)` | 转 "yes"/"no" | `String` |
| `toStringOnOff(bool)` | 转 "on"/"off" | `String` |
| `negate(bool)` | 取反（null→null） | `Boolean` |
| `isTrue(bool)` | 是否 true | `boolean` |
| `isFalse(bool)` | 是否 false | `boolean` |
| `and(values...)` | 逻辑与 | `boolean` |
| `or(values...)` | 逻辑或 | `boolean` |
| `xor(values...)` | 逻辑异或 | `boolean` |

### 示例

```java
import cn.hutool.core.util.BooleanUtil;

// 智能转换
BooleanUtil.toBoolean("yes");    // true
BooleanUtil.toBoolean("1");     // true
BooleanUtil.toBoolean("on");    // true
BooleanUtil.toBoolean("no");    // false

// 布尔转字符串
BooleanUtil.toStringYesNo(true);  // "yes"
BooleanUtil.toStringOnOff(false); // "off"

// 逻辑运算
BooleanUtil.and(true, true, false);  // false
BooleanUtil.or(false, false, true);  // true
BooleanUtil.xor(true, false);        // true

// null 安全取反
BooleanUtil.negate(true);   // false
BooleanUtil.negate(null);   // null
```

---

## DesensitizedUtil — 信息脱敏工具

### 概述

个人隐私信息脱敏工具，支持姓名、身份证、手机号、邮箱、银行卡、地址等多种脱敏规则。适用于日志输出、数据导出、API 响应等场景。

### 核心方法

| 方法名 | 功能 | 示例 |
|--------|------|------|
| `chineseName(name)` | 中文姓名脱敏 | 张三 → 张* |
| `idCardNum(id, front, end)` | 身份证号脱敏 | 11010119...34 → 1101\*\*\*\*\*\*34 |
| `fixedPhone(phone)` | 固定电话脱敏 | 01012345678 → 0101\*\*\*678 |
| `mobilePhone(phone)` | 手机号脱敏 | 13812345678 → 138\*\*\*\*5678 |
| `email(email)` | 邮箱脱敏 | test@163.com → t\*\*@163.com |
| `bankCard(cardNo)` | 银行卡号脱敏 | 6222...1234 → 6222\*\*\*\*\*\*1234 |
| `address(addr, sensitiveSize)` | 地址脱敏 | 北京市朝阳区... → 北京市朝阳区\*\*\* |
| `password(password)` | 密码脱敏 | admin123 → \*\*\*\*\*\*\*\* |
| `carLicense(license)` | 车牌号脱敏 | 京A12345 → 京A1\*\*45 |
| `ipv4(ip)` | IPv4 脱敏 | 192.168.1.1 → 192.\*.\*.\* |
| `firstMask(str)` | 仅显示第一个字符 | 张三 → 张\* |

### 示例

```java
import cn.hutool.core.util.DesensitizedUtil;

// 姓名脱敏
DesensitizedUtil.chineseName("张三丰");
// => "张**"

// 身份证脱敏（保留前4后2）
DesensitizedUtil.idCardNum("110101199001011234", 4, 2);
// => "1101************34"

// 手机号脱敏
DesensitizedUtil.mobilePhone("13812345678");
// => "138****5678"

// 邮箱脱敏
DesensitizedUtil.email("test@example.com");
// => "t***@example.com"

// 银行卡号脱敏
DesensitizedUtil.bankCard("6222021234567890123");
// => "622202*********0123"

// 地址脱敏（隐藏后6位）
DesensitizedUtil.address("北京市朝阳区建国路88号院", 6);
// => "北京市朝阳区建国路******"

// 密码全部脱敏
DesensitizedUtil.password("admin123");
// => "********"

// 车牌号脱敏
DesensitizedUtil.carLicense("京A12345");
// => "京A1**45"

// IP 脱敏
DesensitizedUtil.ipv4("192.168.1.100");
// => "192.*.*.*"
```

> 💡 **使用场景**：日志打印用户信息、API 响应中返回敏感字段、数据导出为 Excel 等。配合 `@Desensitize` 注解可实现 JSON 序列化时自动脱敏。

---

## 常见问题 FAQ

### Q: PageUtil 的页码是从 0 还是 1 开始？
**A**: 默认从 0 开始，可通过 `PageUtil.setFirstPageNo(1)` 改为从 1 开始。

### Q: ZipUtil 解压中文文件名乱码？
**A**: 使用 `ZipUtil.unzip(path, dest, CharsetUtil.CHARSET_GBK)` 指定 GBK 编码。

### Q: DesensitizedUtil 能自定义脱敏规则吗？
**A**: 可以，使用 `StrUtil.hide(str, start, end)` 自定义脱敏位置和范围。

### Q: HashUtil 和 DigestUtil 的区别？
**A**: `HashUtil` 是非加密哈希（用于哈希表、分桶），`DigestUtil`（hutool-crypto）是加密哈希（MD5/SHA 等，用于校验完整性）。

## 最佳实践

1. **分页统一用 `PageUtil`**：避免各处手写 `(pageNo-1)*pageSize`。
2. **日志脱敏用 `DesensitizedUtil`**：GDPR/个人信息保护合规。
3. **文件压缩用 `ZipUtil`**：一行代码搞定压缩解压。
4. **URL 拼接用 `URLUtil.normalize`**：自动处理多余斜杠和协议补全。
5. **布隆过滤器用 `MurmurHash`**：分布均匀，碰撞率低。
