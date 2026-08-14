---
title: POI 与 Excel 工具
classes:
  - cn.hutool.poi.excel.ExcelUtil
  - cn.hutool.poi.excel.ExcelReader
  - cn.hutool.poi.excel.ExcelWriter
  - cn.hutool.poi.excel.BigExcelWriter
module: hutool-poi
verified: 5.8.47
tags: [POI, Excel, XLSX, 导入, 导出, ExcelUtil]
---

# POI 与 Excel 工具

本文档适用于 Hutool 5.8.x，重点覆盖 Excel。`hutool-poi` 是 Apache POI 的便捷封装，不消除 POI 的格式、内存和安全边界。

## 目录

- [入口选择](#入口选择)
- [读取 Excel](#读取-excel)
- [写出 Excel](#写出-excel)
- [大数据量](#大数据量)
- [格式与数据正确性](#格式与数据正确性)
- [依赖与资源管理](#依赖与资源管理)

## 入口选择

| 场景 | API |
|---|---|
| 普通 Excel 读取 | `ExcelUtil.getReader`、`ExcelReader` |
| 普通 Excel 写出 | `ExcelUtil.getWriter`、`ExcelWriter` |
| 大批量 XLSX 写出 | `ExcelUtil.getBigWriter`、`BigExcelWriter` |
| 大文件低内存读取 | `ExcelUtil.readBySax` |

## 读取 Excel

```java
import cn.hutool.poi.excel.ExcelReader;
import cn.hutool.poi.excel.ExcelUtil;

import java.util.List;
import java.util.Map;

try (ExcelReader reader = ExcelUtil.getReader(inputFile, 0)) {
    reader.addHeaderAlias("用户编号", "userId");
    reader.addHeaderAlias("用户名称", "name");
    reader.setIgnoreEmptyRow(true);

    List<Map<String, Object>> rows = reader.readAll();
}
```

表头稳定且 DTO 转换规则明确时，可以使用 `readAll(TargetType.class)`。导入接口仍需逐行校验必填项、长度、枚举、日期和业务唯一性，并返回可定位的行号错误。

## 写出 Excel

```java
import cn.hutool.poi.excel.ExcelUtil;
import cn.hutool.poi.excel.ExcelWriter;

try (ExcelWriter writer = ExcelUtil.getWriter(outputFile, "Users")) {
    writer.addHeaderAlias("userId", "用户编号");
    writer.addHeaderAlias("name", "用户名称");
    writer.setOnlyAlias(true);
    writer.write(users, true);
}
```

写 HTTP 响应时，使用项目统一的文件名编码和响应头策略，再把 Writer 刷入响应流。不要在响应已经提交后继续写错误 JSON；导出前先完成参数和权限校验。

## 大数据量

- `BigExcelWriter` 基于 SXSSF，只支持 XLSX，适合降低大量写出时的内存占用。
- `readBySax` 逐行回调，适合大文件读取；回调内不要无限累积全部行，否则会失去流式处理价值。
- 普通 `ExcelReader` 会持有工作簿。数据量、单元格数量、样式数量和图片都可能影响内存，不能只按文件字节大小判断。
- 批量落库时使用受控批次，记录失败行，并明确整批回滚还是部分成功。

SAX 入口示意：

```java
ExcelUtil.readBySax(inputFile, 0, (sheetIndex, rowIndex, rowList) -> {
    importRow(rowIndex, rowList);
});
```

## 格式与数据正确性

- Excel 数值可能被 POI 读为浮点类型。身份证号、订单号、银行卡号等应按文本读取，避免科学计数法、前导零丢失和精度变化。
- 日期单元格本质上可能是数字加格式；明确时区、日期格式和空单元格行为。
- 公式单元格要决定读取公式文本还是计算结果；不要默认外部工作簿的缓存结果可信。
- CSV 不是 Excel 工作簿，不要用 POI 入口假装解析；使用明确字符集和转义规则的 CSV 方案。
- 导出到电子表格时，用户可控且以 `=`, `+`, `-`, `@` 开头的文本可能触发公式注入，应按业务策略转义。
- 不可信 Office 文件可能造成压缩炸弹或资源耗尽；限制上传大小、行列数、处理时长，并使用受支持的 POI 版本。

## 依赖与资源管理

所属模块为 `cn.hutool:hutool-poi`。Hutool 5.x 将 `poi-ooxml` 等实现依赖声明为可选项；项目通常还需要显式提供与当前 Hutool 版本兼容的 Apache POI 依赖。先检查依赖树和项目 BOM，不要随意混搭 POI 版本。

`ExcelReader`、`ExcelWriter` 和 `BigExcelWriter` 都应关闭。使用 try-with-resources；对输入流、输出流的所有权也要明确，避免重复关闭框架管理的响应流。

官方 API：[ExcelUtil](https://plus.hutool.cn/apidocs/cn/hutool/poi/excel/ExcelUtil.html)、[ExcelReader](https://plus.hutool.cn/apidocs/cn/hutool/poi/excel/ExcelReader.html)、[ExcelWriter](https://plus.hutool.cn/apidocs/cn/hutool/poi/excel/ExcelWriter.html)。
