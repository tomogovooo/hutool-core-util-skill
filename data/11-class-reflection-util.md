---
title: 类与反射工具
classes:
  - cn.hutool.core.util.ClassUtil
  - cn.hutool.core.util.ReflectUtil
since: 3.0.0
tags: [类, class, 反射, reflection, 类加载, ClassUtil, ReflectUtil]
---

# 类与反射工具 — ClassUtil / ReflectUtil

## 概述

- **`ClassUtil`**：类工具，提供类名获取、类型判断、包扫描、泛型参数获取等功能。
- **`ReflectUtil`**：反射工具，封装了 Java 反射 API 的常用操作，简化对象创建、方法调用、字段访问等反射操作。

## Maven 依赖

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-core</artifactId>
    <version>5.8.25</version>
</dependency>
```

## 常用方法速查表

### ClassUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `getClassName(obj, isSimple)` | 获取类名 | `String` | 3.0+ |
| `getShortClassName(className)` | 获取短类名 | `String` | 4.0+ |
| `isAssignable(targetType, sourceType)` | 是否可赋值 | `boolean` | 3.0+ |
| `isBasicType(clazz)` | 是否基本类型 | `boolean` | 3.0+ |
| `isPrimitiveWrapper(clazz)` | 是否包装类 | `boolean` | 3.0+ |
| `isAbstract(clazz)` | 是否抽象类 | `boolean` | 5.4+ |
| `isNormalClass(clazz)` | 是否普通类 | `boolean` | 5.4+ |
| `isEnum(clazz)` | 是否枚举 | `boolean` | 5.4+ |
| `isInterface(clazz)` | 是否接口 | `boolean` | 5.4+ |
| `getPackage(clazz)` | 获取包名 | `String` | 3.0+ |
| `getPackagePath(clazz)` | 获取包路径 | `String` | 3.0+ |
| `scanPackage(packageName)` | 扫描包下所有类 | `Set<Class<?>>` | 3.0+ |
| `scanPackageByAnnotation(pkg, annotation)` | 按注解扫描 | `Set<Class<?>>` | 4.0+ |
| `scanPackageBySuper(pkg, superClass)` | 按父类扫描 | `Set<Class<?>>` | 4.0+ |
| `getClassPaths()` | 获取 ClassPath | `Set<String>` | 3.0+ |
| `getJavaClassPaths()` | 获取 Java ClassPath | `String[]` | 3.0+ |
| `loadClass(className)` | 加载类 | `Class<?>` | 3.0+ |
| `getDefaultValue(clazz)` | 获取类型默认值 | `Object` | 5.4+ |
| `getPublicMethods(clazz)` | 获取公共方法 | `Method[]` | 4.0+ |
| `getTypeArgument(clazz)` | 获取泛型参数 | `Type` | 3.0+ |
| `getTypeArgument(clazz, index)` | 获取指定位置泛型参数 | `Type` | 3.0+ |

### ReflectUtil

| 方法名 | 功能 | 返回类型 | 版本 |
|--------|------|----------|------|
| `newInstance(clazz, params...)` | 反射创建实例 | `T` | 3.0+ |
| `newInstanceIfPossible(clazz)` | 尝试创建实例 | `T` | 5.4+ |
| `getMethod(clazz, name, paramTypes...)` | 获取方法 | `Method` | 3.0+ |
| `getMethodByName(clazz, name)` | 按名称获取方法 | `Method` | 5.4+ |
| `getMethods(clazz)` | 获取所有方法 | `Method[]` | 3.0+ |
| `invoke(obj, methodName, args...)` | 反射调用方法 | `T` | 3.0+ |
| `invokeStatic(method, args...)` | 反射调用静态方法 | `T` | 5.4+ |
| `getField(clazz, fieldName)` | 获取字段 | `Field` | 3.0+ |
| `getFields(clazz)` | 获取所有字段 | `Field[]` | 3.0+ |
| `getFieldsDirectly(clazz, withSuper)` | 获取字段（含/不含父类） | `Field[]` | 5.4+ |
| `getFieldValue(obj, fieldName)` | 获取字段值 | `Object` | 3.0+ |
| `getFieldsValue(obj)` | 获取所有字段值 | `Object[]` | 5.0+ |
| `setFieldValue(obj, fieldName, value)` | 设置字段值 | `void` | 3.0+ |
| `getConstructor(clazz, paramTypes...)` | 获取构造器 | `Constructor<T>` | 5.4+ |
| `getConstructors(clazz)` | 获取所有构造器 | `Constructor<?>[]` | 5.4+ |
| `setAccessible(accessibleObj)` | 设置可访问 | `T` | 3.0+ |

## 详细 API 与示例

### ClassUtil — 类名操作

```java
import cn.hutool.core.util.ClassUtil;

// 获取类名
ClassUtil.getClassName(new ArrayList<>(), false);  // "java.util.ArrayList"
ClassUtil.getClassName(new ArrayList<>(), true);   // "ArrayList"

// 短类名
ClassUtil.getShortClassName("cn.hutool.core.util.StrUtil");  // "c.h.c.u.StrUtil"

// 包名
ClassUtil.getPackage(StrUtil.class);      // "cn.hutool.core.util"
ClassUtil.getPackagePath(StrUtil.class);   // "cn/hutool/core/util"
```

### ClassUtil — 类型判断

```java
import cn.hutool.core.util.ClassUtil;

// 基本类型判断
ClassUtil.isBasicType(int.class);           // true
ClassUtil.isBasicType(Integer.class);       // true（包含包装类）
ClassUtil.isPrimitiveWrapper(Integer.class); // true
ClassUtil.isPrimitiveWrapper(int.class);    // false

// 类型分类
ClassUtil.isAbstract(AbstractList.class);   // true
ClassUtil.isEnum(DayOfWeek.class);          // true
ClassUtil.isInterface(List.class);          // true
ClassUtil.isNormalClass(ArrayList.class);   // true

// 类型兼容性
ClassUtil.isAssignable(Number.class, Integer.class);  // true（Integer 可赋值给 Number）
```

### ClassUtil — 包扫描

```java
import cn.hutool.core.util.ClassUtil;

// 扫描包下所有类
Set<Class<?>> classes = ClassUtil.scanPackage("cn.hutool.core.util");

// 按注解扫描（如找所有 @Component）
Set<Class<?>> components = ClassUtil.scanPackageByAnnotation(
    "com.myapp", Component.class);

// 按父类/接口扫描
Set<Class<?>> services = ClassUtil.scanPackageBySuper(
    "com.myapp.service", BaseService.class);
```

> ⚠️ **性能注意**：包扫描会遍历 classpath 下的所有类文件，大型项目中应尽量缩小扫描范围，避免在运行时频繁调用。

### ClassUtil — 泛型参数

```java
import cn.hutool.core.util.ClassUtil;

// 获取父类的泛型参数
// 假设 UserService extends BaseService<User, Long>
Type type = ClassUtil.getTypeArgument(UserService.class);       // User
Type type2 = ClassUtil.getTypeArgument(UserService.class, 1);   // Long
```

### ReflectUtil — 创建实例

```java
import cn.hutool.core.util.ReflectUtil;

// 反射创建实例
ArrayList<String> list = ReflectUtil.newInstance(ArrayList.class);

// 带参构造
StringBuilder sb = ReflectUtil.newInstance(StringBuilder.class, "Hello");

// 尝试创建（失败返回 null，不抛异常）
Object obj = ReflectUtil.newInstanceIfPossible(SomeClass.class);
```

### ReflectUtil — 方法调用

```java
import cn.hutool.core.util.ReflectUtil;

// 反射调用方法
String str = "Hello World";
// 调用 String.substring(int, int)
String sub = ReflectUtil.invoke(str, "substring", 0, 5);
// => "Hello"

// 获取方法对象
Method method = ReflectUtil.getMethod(String.class, "length");
Method method2 = ReflectUtil.getMethodByName(String.class, "trim");

// 调用静态方法
Integer max = ReflectUtil.invokeStatic(
    ReflectUtil.getMethod(Math.class, "max", int.class, int.class),
    3, 5);
// => 5
```

### ReflectUtil — 字段操作

```java
import cn.hutool.core.util.ReflectUtil;

// 获取字段值（即使是 private）
User user = new User("张三", 25);
Object name = ReflectUtil.getFieldValue(user, "name");     // "张三"

// 设置字段值（即使是 private）
ReflectUtil.setFieldValue(user, "name", "李四");

// 获取所有字段值
Object[] values = ReflectUtil.getFieldsValue(user);

// 获取字段（含父类字段）
Field[] fields = ReflectUtil.getFieldsDirectly(User.class, true);

// 只获取当前类字段
Field[] directFields = ReflectUtil.getFieldsDirectly(User.class, false);
```

### ReflectUtil — 设置可访问

```java
import cn.hutool.core.util.ReflectUtil;

// 设置 private 成员可访问
Field field = ReflectUtil.getField(User.class, "name");
ReflectUtil.setAccessible(field);

Method method = ReflectUtil.getMethod(User.class, "privateMethod");
ReflectUtil.setAccessible(method);
```

## 常见问题 FAQ

### Q: 反射操作性能如何？
**A**: 反射比直接调用慢 5~50 倍。低频操作（初始化、配置加载）没问题；**高频操作应避免反射**，考虑使用缓存 Method/Field 或编译期代码生成（如 MapStruct）。

### Q: Java 9+ 模块系统下反射受限吗？
**A**: 是的，Java 9+ 的模块系统（JPMS）限制了对未开放模块的反射访问。可能需要 `--add-opens` JVM 参数。Hutool 内部已尽量处理了 `setAccessible` 的异常。

### Q: scanPackage 能扫描 JAR 包内的类吗？
**A**: 可以，`scanPackage` 支持扫描 classpath 上的 JAR 文件和文件目录。

### Q: getFieldsDirectly 的 withSuper 参数？
**A**: `true` 时包含所有父类的字段；`false` 仅当前类声明的字段。不含 Object 类的字段。

## 最佳实践

1. **类型判断用 `ClassUtil`**：比直接用 `Modifier.isAbstract` 等更简洁。
2. **包扫描用 `scanPackageByAnnotation`**：自定义组件扫描逻辑时替代 Spring 扫描。
3. **反射调用缓存 Method**：频繁调用时先 `getMethod` 缓存，再 `invoke`。
4. **字段值批量获取用 `getFieldsValue`**：适合序列化、日志等场景。
5. **获取泛型参数用 `getTypeArgument`**：框架开发中自动推断泛型类型。
6. **生产环境谨慎使用 `setFieldValue`**：修改 private 字段打破封装，维护成本高。
