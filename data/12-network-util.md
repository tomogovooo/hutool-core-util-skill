---
title: 网络工具
classes:
  - cn.hutool.core.net.NetUtil
since: 3.0.0
tags: [网络, network, IP, 端口, port, NetUtil]
---

# 网络工具 — NetUtil

## 概述

`NetUtil` 提供了网络相关的常用工具方法，包括本机 IP/MAC 地址获取、端口检测、IP 格式转换、内外网判断、Ping 等功能。适用于服务注册、健康检查、网络诊断等场景。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `getLocalhostStr()` | 获取本机 IP 地址字符串 | `String` | 3.0+ |
| `getLocalhost()` | 获取本机 InetAddress | `InetAddress` | 3.0+ |
| `getLocalMacAddress()` | 获取本机 MAC 地址 | `String` | 4.0+ |
| `getLocalHostName()` | 获取本机主机名 | `String` | 4.0+ |
| `isInnerIP(ip)` | 是否内网 IP | `boolean` | 3.0+ |
| `isValidPort(port)` | 端口是否合法（0~65535） | `boolean` | 5.4+ |
| `isUsableLocalPort(port)` | 本地端口是否可用 | `boolean` | 3.0+ |
| `getUsableLocalPort()` | 获取一个可用端口 | `int` | 3.0+ |
| `getUsableLocalPort(minPort)` | 从指定端口开始查找可用端口 | `int` | 5.4+ |
| `longToIpv4(longIp)` | long 转 IPv4 字符串 | `String` | 3.0+ |
| `ipv4ToLong(ip)` | IPv4 字符串转 long | `long` | 3.0+ |
| `ping(ip)` | Ping 指定主机 | `boolean` | 4.0+ |
| `ping(ip, timeout)` | 带超时 Ping | `boolean` | 4.0+ |
| `getMultistageReverseProxyIp(ip)` | 获取多级代理后的真实 IP | `String` | 4.0+ |
| `isInRange(ip, cidr)` | IP 是否在指定网段内 | `boolean` | 5.4+ |
| `hideIpPart(ip)` | 隐藏 IP 部分段 | `String` | 5.4+ |
| `buildInetSocketAddress(host, port)` | 构建 InetSocketAddress | `InetSocketAddress` | 4.0+ |
| `parseCookies(cookieStr)` | 解析 Cookie 字符串 | `List<HttpCookie>` | 5.4+ |
| `toAbsoluteUrl(absoluteBasePath, relativePath)` | 转绝对 URL | `String` | 5.4+ |
| `LOCAL_IP` | 本地IP常量 127.0.0.1 | `String` | 3.0+ |

## 详细 API 与示例

### 获取本机网络信息

```java
import cn.hutool.core.net.NetUtil;

// 获取本机 IP（会过滤虚拟网卡等，取第一个有效 IP）
String ip = NetUtil.getLocalhostStr();
// => "192.168.1.100"

// 获取 InetAddress 对象
InetAddress addr = NetUtil.getLocalhost();

// 获取主机名
String hostname = NetUtil.getLocalHostName();
// => "MY-PC"

// 获取 MAC 地址
String mac = NetUtil.getLocalMacAddress();
// => "AA-BB-CC-DD-EE-FF"
```

> ⚠️ **多网卡环境**：`getLocalhostStr()` 会尝试返回非回环、非虚拟网卡的第一个有效 IP。多网卡时可能不是期望的 IP，可通过配置或环境变量指定。

### 内外网判断

```java
import cn.hutool.core.net.NetUtil;

// 判断是否内网 IP
NetUtil.isInnerIP("192.168.1.1");    // true（C类私有）
NetUtil.isInnerIP("10.0.0.1");       // true（A类私有）
NetUtil.isInnerIP("172.16.0.1");     // true（B类私有）
NetUtil.isInnerIP("127.0.0.1");      // true（回环）
NetUtil.isInnerIP("8.8.8.8");        // false（公网）
```

> 💡 **私有地址范围**：
> - A 类：`10.0.0.0 ~ 10.255.255.255`
> - B 类：`172.16.0.0 ~ 172.31.255.255`
> - C 类：`192.168.0.0 ~ 192.168.255.255`
> - 回环：`127.0.0.0 ~ 127.255.255.255`

### 端口检测

```java
import cn.hutool.core.net.NetUtil;

// 端口合法性
NetUtil.isValidPort(8080);    // true
NetUtil.isValidPort(-1);      // false
NetUtil.isValidPort(70000);   // false

// 端口是否可用（未被占用）
NetUtil.isUsableLocalPort(8080);  // true/false

// 获取一个可用端口
int port = NetUtil.getUsableLocalPort();        // 如 54321
int port2 = NetUtil.getUsableLocalPort(9000);   // 从 9000 开始查找
```

### IP 格式转换

```java
import cn.hutool.core.net.NetUtil;

// IPv4 ↔ long 互转
long ipLong = NetUtil.ipv4ToLong("192.168.1.100");
// => 3232235876

String ipStr = NetUtil.longToIpv4(3232235876L);
// => "192.168.1.100"
```

### IP 网段判断

```java
import cn.hutool.core.net.NetUtil;

// 判断 IP 是否在 CIDR 网段内
NetUtil.isInRange("192.168.1.100", "192.168.1.0/24");   // true
NetUtil.isInRange("192.168.2.100", "192.168.1.0/24");   // false
NetUtil.isInRange("10.0.0.50", "10.0.0.0/8");           // true
```

### Ping

```java
import cn.hutool.core.net.NetUtil;

// Ping 主机（默认超时）
boolean reachable = NetUtil.ping("192.168.1.1");

// 带超时的 Ping（毫秒）
boolean reachable2 = NetUtil.ping("8.8.8.8", 3000);
```

> ⚠️ **注意**：`ping` 方法底层使用 `InetAddress.isReachable()`，某些系统/网络环境可能需要管理员权限。Windows 下默认使用 ICMP，Linux 下可能回退到 TCP 端口 7。

### IP 脱敏 / 代理 IP

```java
import cn.hutool.core.net.NetUtil;

// 隐藏 IP 部分段
String hidden = NetUtil.hideIpPart("192.168.1.100");
// => "192.168.1.*"

// 获取多级反向代理后的真实 IP
// 常用于从 X-Forwarded-For 头中提取
String realIp = NetUtil.getMultistageReverseProxyIp(
    "10.0.0.1, 192.168.1.1, 172.16.0.1");
// => "10.0.0.1"（取第一个非未知 IP）
```

### 其他工具

```java
import cn.hutool.core.net.NetUtil;

// 构建 InetSocketAddress
InetSocketAddress address = NetUtil.buildInetSocketAddress("127.0.0.1", 8080);

// 解析 Cookie 字符串
List<HttpCookie> cookies = NetUtil.parseCookies(
    "name=value; sessionId=abc123; token=xyz");

// 拼接绝对 URL
String absUrl = NetUtil.toAbsoluteUrl(
    "https://example.com/api/", "../images/logo.png");
// => "https://example.com/images/logo.png"

// 本地 IP 常量
String localIp = NetUtil.LOCAL_IP;  // "127.0.0.1"
```

## 常见问题 FAQ

### Q: getLocalhostStr 在 Docker 容器内返回什么？
**A**: 通常返回容器的虚拟网卡 IP（如 `172.17.0.2`）。如需获取宿主机 IP，需要通过环境变量传入。

### Q: 多网卡环境怎么获取正确的 IP？
**A**: `getLocalhostStr` 的选择逻辑可能不满足需求。可以通过 `NetworkInterface.getNetworkInterfaces()` 遍历所有网卡，自行选择。

### Q: isUsableLocalPort 的实现原理？
**A**: 尝试绑定该端口的 ServerSocket，成功则可用，失败（`BindException`）则被占用。

### Q: ping 返回 false 一定是不通吗？
**A**: 不一定。防火墙可能阻止 ICMP，或 Java 权限不足。`ping` 返回 false 只表示 `isReachable()` 失败。

## 最佳实践

1. **服务注册用 `getLocalhostStr`**：自动获取本机 IP 注册到 Nacos/Eureka。
2. **动态端口用 `getUsableLocalPort`**：测试或临时服务自动分配端口。
3. **安全审计用 `isInnerIP`**：区分内外网请求，内网接口限制公网访问。
4. **日志脱敏用 `hideIpPart`**：日志中不暴露完整 IP。
5. **反代场景用 `getMultistageReverseProxyIp`**：从 `X-Forwarded-For` 获取真实客户端 IP。
6. **IP 存储用 long**：`ipv4ToLong` 转换后存储为整数，查询效率更高。
