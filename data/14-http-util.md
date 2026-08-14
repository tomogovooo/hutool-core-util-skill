---
title: HTTP 请求工具
classes:
  - cn.hutool.http.HttpUtil
  - cn.hutool.http.HttpRequest
  - cn.hutool.http.HttpResponse
module: hutool-http
verified: 5.8.47
tags: [HTTP, HTTPS, 表单, 上传, 下载, HttpUtil, HttpRequest]
---

# HTTP 请求工具

本文档适用于 Hutool 5.8.x。Hutool 6.x 的坐标、包名和签名可能不同，只把本文当作能力线索，并按项目精确版本重新核实。

## 目录

- [选择入口](#选择入口)
- [GET 与 POST](#get-与-post)
- [自定义请求](#自定义请求)
- [上传与下载](#上传与下载)
- [资源与安全边界](#资源与安全边界)
- [依赖与替换判断](#依赖与替换判断)

## 选择入口

| 场景 | 优先入口 | 说明 |
|---|---|---|
| 简单 GET/POST，直接取字符串 | `HttpUtil` | 一次性快捷调用 |
| Header、认证、超时、请求体或代理 | `HttpRequest` | 链式构造请求 |
| 检查状态码、响应头、字节或流 | `HttpResponse` | 使用后必须关闭 |
| 文件下载 | `HttpUtil.downloadFile*` 或响应流 | 先确认大小、目标路径和覆盖策略 |

不要用同步 `hutool-http` 替换 Reactor、协程或框架异步客户端；执行模型不同。

## GET 与 POST

```java
import cn.hutool.http.HttpUtil;

import java.util.HashMap;
import java.util.Map;

Map<String, Object> query = new HashMap<>();
query.put("page", 1);
query.put("size", 20);

String body = HttpUtil.get("https://api.example.com/users", query, 5_000);
String result = HttpUtil.post("https://api.example.com/form", query, 5_000);
```

`HttpUtil.get/post` 适合无需读取状态码和响应头的简单请求。只要成功与失败需要不同处理，就改用 `HttpRequest`。

## 自定义请求

```java
import cn.hutool.http.ContentType;
import cn.hutool.http.Header;
import cn.hutool.http.HttpRequest;
import cn.hutool.http.HttpResponse;
import cn.hutool.json.JSONUtil;

String requestJson = JSONUtil.toJsonStr(payload);

try (HttpResponse response = HttpRequest.post("https://api.example.com/orders")
        .header(Header.ACCEPT, ContentType.JSON.getValue())
        .contentType(ContentType.JSON.getValue())
        .bearerAuth(accessToken)
        .body(requestJson)
        .timeout(5_000)
        .execute()) {
    if (!response.isOk()) {
        throw new IllegalStateException("HTTP status: " + response.getStatus());
    }
    String responseJson = response.body();
}
```

常用能力：

- `HttpRequest.get/post/put/patch/delete/head(url)`：选择请求方法。
- `header`、`contentType`、`bearerAuth`、`basicAuth`：设置请求元数据和认证。
- `form(Map)`、`form(name, file)`：表单或文件字段。
- `body(String)`：原始请求体；同时显式给出正确 Content-Type。
- `timeout(int)`：设置超时；值应来自项目配置，不要永久等待。
- `execute()`：同步取得 `HttpResponse`；`executeAsync()` 只是延迟读取响应体，不等于非阻塞客户端。

## 上传与下载

```java
try (HttpResponse response = HttpRequest.post(uploadUrl)
        .form("description", "monthly report")
        .form("file", reportFile)
        .timeout(30_000)
        .execute()) {
    if (!response.isOk()) {
        throw new IllegalStateException("Upload failed: " + response.getStatus());
    }
}
```

下载大文件时优先把 `bodyStream()` 写入受控目标，不要先用 `bodyBytes()` 把未知大小响应全部放进内存。校验 Content-Length 不能代替实际读取上限，因为服务端可能缺失或伪造该响应头。

## 资源与安全边界

- `HttpResponse` 实现 `AutoCloseable`；使用 try-with-resources，响应流也由调用方负责正确关闭。
- 检查 `getStatus()` 或 `isOk()`；成功建立连接不代表业务成功。
- 对连接和读取设置合理超时，并按接口幂等性决定是否重试。不要自动重试非幂等写请求。
- 用户可控 URL 会形成 SSRF 风险。限制协议、主机、端口和重定向目标，并阻止访问内网与云元数据地址。
- 不要开启宽松 TLS、跳过证书或主机名校验；敏感 Header、Cookie、令牌和响应正文不要直接写日志。
- 限制响应、上传和下载大小；下载路径必须经过目录约束，避免覆盖任意文件。
- 认证、签名、代理、Cookie 和重定向策略应服从项目统一 HTTP 封装。

## 依赖与替换判断

所属模块为 `cn.hutool:hutool-http`；使用 `hutool-all` 时仍需确认版本。Hutool 5.x 的 HTTP 模块会使用 Core，并可结合 JSON 模块，但不要仅因已有 `hutool-core` 就直接生成这些 import。

适合替换重复的 `HttpURLConnection` 样板代码；不适合越过 Spring `WebClient`、Feign、OkHttp 拦截器或项目统一的鉴权、追踪、熔断与连接池策略。

官方 API：[HttpUtil](https://plus.hutool.cn/apidocs/cn/hutool/http/HttpUtil.html)、[HttpRequest](https://plus.hutool.cn/apidocs/cn/hutool/http/HttpRequest.html)、[HttpResponse](https://plus.hutool.cn/apidocs/cn/hutool/http/HttpResponse.html)。
