---
title: IO与文件工具
classes:
  - cn.hutool.core.io.IoUtil
  - cn.hutool.core.io.FileUtil
  - cn.hutool.core.io.resource.ResourceUtil
since: 3.0.0
tags: [IO, 文件, file, 流, stream, resource, 资源, FileUtil, IoUtil]
---

# IO与文件工具 — FileUtil / IoUtil / ResourceUtil

## 概述

- **`FileUtil`**：文件工具类，封装了文件/目录的创建、读写、复制、删除、遍历、路径操作等。一站式解决文件相关需求。
- **`IoUtil`**：IO 流工具类，提供流的读写、复制、安全关闭等操作。
- **`ResourceUtil`**：资源工具类，用于读取 classpath 下的资源文件。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### FileUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `readUtf8String(file)` | 读取文件内容为 UTF-8 字符串 | `String` | 3.0+ |
| `readString(file, charset)` | 指定编码读取 | `String` | 3.0+ |
| `readBytes(file)` | 读取为字节数组 | `byte[]` | 3.0+ |
| `readUtf8Lines(file)` | 按行读取为 List | `List<String>` | 3.0+ |
| `readLines(file, charset)` | 指定编码按行读取 | `List<String>` | 3.0+ |
| `writeUtf8String(content, file)` | UTF-8 写入文件 | `File` | 3.0+ |
| `writeString(content, file, charset)` | 指定编码写入 | `File` | 3.0+ |
| `writeBytes(data, file)` | 写入字节数组 | `File` | 3.0+ |
| `appendUtf8String(content, file)` | UTF-8 追加写入 | `File` | 4.0+ |
| `appendString(content, file, charset)` | 指定编码追加 | `File` | 4.0+ |
| `copy(src, dest, isOverride)` | 复制文件/目录 | `File` | 3.0+ |
| `move(src, dest, isOverride)` | 移动文件/目录 | `void` | 3.0+ |
| `rename(file, newName, isOverride)` | 重命名 | `File` | 3.0+ |
| `del(file)` | 删除文件/目录（递归） | `boolean` | 3.0+ |
| `mkdir(dir)` | 创建目录（含父目录） | `File` | 3.0+ |
| `mkParentDirs(file)` | 创建父目录 | `File` | 5.4+ |
| `touch(file)` | 创建文件（含父目录） | `File` | 3.0+ |
| `file(path)` | 字符串转 File 对象 | `File` | 3.0+ |
| `exist(path)` | 文件/目录是否存在 | `boolean` | 3.0+ |
| `isFile(path)` | 是否为文件 | `boolean` | 3.0+ |
| `isDirectory(path)` | 是否为目录 | `boolean` | 3.0+ |
| `ls(dir)` | 列出目录下的文件 | `String[]` | 4.0+ |
| `loopFiles(dir)` | 递归列出所有文件 | `List<File>` | 3.0+ |
| `loopFiles(dir, fileFilter)` | 带过滤的递归列出 | `List<File>` | 3.0+ |
| `size(file)` | 文件大小（字节） | `long` | 3.0+ |
| `readableFileSize(size)` | 可读的文件大小 | `String` | 3.0+ |
| `getName(path)` | 获取文件名 | `String` | 3.0+ |
| `getPrefix(file)` | 获取主文件名（不含扩展名） | `String` | 4.0+ |
| `getSuffix(file)` | 获取扩展名 | `String` | 4.0+ |
| `extName(fileName)` | 获取扩展名 | `String` | 3.0+ |
| `getTmpDir()` | 获取临时目录 | `File` | 4.0+ |
| `getUserHomeDir()` | 获取用户主目录 | `File` | 4.0+ |
| `isAbsolutePath(path)` | 是否绝对路径 | `boolean` | 5.0+ |
| `normalize(path)` | 路径标准化 | `String` | 3.0+ |
| `tail(file, charset)` | 文件尾部追踪 | `void` | 4.0+ |

### IoUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `read(input, charset)` | 从流读取为字符串 | `String` | 3.0+ |
| `readUtf8(input)` | UTF-8 读取流 | `String` | 3.0+ |
| `readBytes(input)` | 读取为字节数组 | `byte[]` | 3.0+ |
| `write(output, charset, content)` | 写入流 | `void` | 3.0+ |
| `writeUtf8(output, content)` | UTF-8 写入流 | `void` | 3.0+ |
| `copy(input, output)` | 流拷贝 | `long` | 3.0+ |
| `close(closeable)` | 安全关闭（不抛异常） | `void` | 3.0+ |
| `toStream(str, charset)` | 字符串转流 | `InputStream` | 3.0+ |
| `toBuffered(input/output)` | 转缓冲流 | `Buffered*` | 3.0+ |
| `readObj(input)` | 反序列化对象 | `T` | 3.0+ |
| `writeObj(output, obj)` | 序列化对象 | `void` | 3.0+ |

### ResourceUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `readUtf8Str(resource)` | 读取 classpath 资源为字符串 | `String` | 3.0+ |
| `readStr(resource, charset)` | 指定编码读取 | `String` | 3.0+ |
| `readBytes(resource)` | 读取为字节数组 | `byte[]` | 4.0+ |
| `getResource(resource)` | 获取资源 URL | `URL` | 3.0+ |
| `getResourceObj(resource)` | 获取资源对象 | `Resource` | 5.0+ |
| `getStream(resource)` | 获取资源流 | `InputStream` | 4.0+ |

## 详细 API 与示例

### FileUtil — 文件读写

```java
import cn.hutool.core.io.FileUtil;

// 读取文件内容
String content = FileUtil.readUtf8String("d:/test.txt");

// 按行读取
List<String> lines = FileUtil.readUtf8Lines("d:/test.txt");

// 写入文件（覆盖）
FileUtil.writeUtf8String("Hello Hutool", "d:/output.txt");

// 追加写入
FileUtil.appendUtf8String("追加内容\n", "d:/output.txt");

// 写入字节数组
FileUtil.writeBytes(imageBytes, "d:/image.png");
```

### FileUtil — 文件/目录操作

```java
import cn.hutool.core.io.FileUtil;

// 创建目录（含父目录）
FileUtil.mkdir("d:/a/b/c");

// 创建文件（自动创建父目录）
FileUtil.touch("d:/a/b/test.txt");

// 复制文件
FileUtil.copy("d:/source.txt", "d:/backup/source.txt", true);

// 复制目录
FileUtil.copy("d:/mydir", "d:/backup/mydir", true);

// 移动文件
FileUtil.move(FileUtil.file("d:/old.txt"), FileUtil.file("d:/new/old.txt"), true);

// 重命名
FileUtil.rename(FileUtil.file("d:/old.txt"), "new.txt", true);

// 删除（目录会递归删除）
FileUtil.del("d:/temp");
```

### FileUtil — 文件判断与信息

```java
import cn.hutool.core.io.FileUtil;

// 存在性判断
FileUtil.exist("d:/test.txt");       // true/false
FileUtil.isFile("d:/test.txt");      // true
FileUtil.isDirectory("d:/mydir");    // true

// 文件大小
long size = FileUtil.size(FileUtil.file("d:/large.zip"));
String readable = FileUtil.readableFileSize(size);  // "1.5 GB"

// 文件名操作
FileUtil.getName("d:/path/to/file.txt");     // "file.txt"
FileUtil.getPrefix(FileUtil.file("file.txt")); // "file"
FileUtil.getSuffix(FileUtil.file("file.txt")); // "txt"
FileUtil.extName("archive.tar.gz");            // "gz"

// 路径操作
FileUtil.normalize("d:/path/../other/./file");  // "d:/other/file"
FileUtil.isAbsolutePath("d:/test");              // true
FileUtil.isAbsolutePath("relative/path");        // false
```

### FileUtil — 遍历目录

```java
import cn.hutool.core.io.FileUtil;

// 列出目录下直接子项
String[] names = FileUtil.ls("d:/mydir");

// 递归列出所有文件
List<File> allFiles = FileUtil.loopFiles("d:/mydir");

// 带过滤的递归列出（如只要 .java 文件）
List<File> javaFiles = FileUtil.loopFiles("d:/project",
    file -> file.getName().endsWith(".java"));
```

### FileUtil — 系统目录

```java
import cn.hutool.core.io.FileUtil;

File tmpDir = FileUtil.getTmpDir();        // 临时目录
File homeDir = FileUtil.getUserHomeDir();  // 用户主目录
```

### IoUtil — 流操作

```java
import cn.hutool.core.io.IoUtil;

// 从流读取
InputStream in = new FileInputStream("d:/test.txt");
String content = IoUtil.readUtf8(in);
IoUtil.close(in);  // 安全关闭

// 流拷贝
InputStream input = ...;
OutputStream output = ...;
long copied = IoUtil.copy(input, output);
IoUtil.close(input);
IoUtil.close(output);

// 安全关闭（不抛异常，null 安全）
IoUtil.close(null);  // 不会 NPE
```

### ResourceUtil — 读取 classpath 资源

```java
import cn.hutool.core.io.resource.ResourceUtil;

// 读取 classpath 下的配置文件
String config = ResourceUtil.readUtf8Str("config/app.properties");

// 读取模板文件
String template = ResourceUtil.readUtf8Str("templates/email.html");

// 获取资源 URL
URL url = ResourceUtil.getResource("config/app.properties");

// 获取资源流
InputStream stream = ResourceUtil.getStream("data/init.sql");
```

## 常见问题 FAQ

### Q: FileUtil.readUtf8String 和 IoUtil.readUtf8 的区别？
**A**: `FileUtil` 接受文件路径/File 对象，自动处理流的打开和关闭；`IoUtil` 接受已打开的 `InputStream`，需要手动管理流。

### Q: FileUtil.del 能删除非空目录吗？
**A**: 可以，`del` 会递归删除目录下所有文件和子目录。**谨慎使用！**

### Q: ResourceUtil 读取不到资源怎么办？
**A**: 确保资源文件在 classpath 下（如 `src/main/resources`），且路径不要以 `/` 开头（相对路径）。打包后确认资源在 JAR 内。

### Q: 写入文件时目录不存在会怎样？
**A**: `writeUtf8String` 等方法会自动创建父目录，无需手动 `mkdir`。

### Q: IoUtil.close 和 try-with-resources 哪个好？
**A**: Java 7+ 推荐 try-with-resources。`IoUtil.close` 适合无法使用 try-with-resources 的场景（如 finally 块中关闭多个流）。

## 最佳实践

1. **文件读写首选 `FileUtil`**：自动管理流生命周期，比手动操作 `FileInputStream` 简洁很多。
2. **指定编码**：始终使用 `readUtf8String` 而非 `readString`（除非确定文件编码不是 UTF-8）。
3. **大文件用流式处理**：`readUtf8String` 会把整个文件加载到内存，大文件（>100MB）应使用 `readLines` 或流式 API。
4. **递归删除需谨慎**：`FileUtil.del` 递归删除不可恢复，建议先 `loopFiles` 确认文件列表。
5. **classpath 资源用 `ResourceUtil`**：不要手动拼接 `getClass().getResource()` 路径。
6. **路径标准化用 `normalize`**：解决 `..` 和 `./` 导致的路径问题。
