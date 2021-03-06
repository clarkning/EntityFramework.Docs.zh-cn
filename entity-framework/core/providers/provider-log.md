---
title: 提供程序影响的更改-EF Core 的日志
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 44b200223153fca44cb2cfa3e78b3bedc7b4a552
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821330"
---
# <a name="provider-impacting-changes"></a>提供程序影响的更改

此页包含用于拉取请求可能需要的其他数据库提供程序以响应作者在 EF Core 存储库进行链接。 目的是要更新到新版本的提供商时，适用于现有第三方数据库提供程序的作者提供一个起始点。

我们此日志着手从 2.1 到 2.2 的更改。 在 2.1 之前，我们使用[ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware)并[ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi)上我们的问题和拉取请求标签。

### <a name="21-----22"></a>2.1---> 2.2

#### <a name="test-only-changes"></a>仅限测试的更改

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -在测试中允许可自定义 SQL 分隔符
  * 测试 BuiltInDataTypesTestBase 中允许非严格的浮动点比较的更改
  * 测试更改，可查询的测试以重新用于其他 SQL 分隔符
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -将 DbFunction 测试添加到关系规范测试
  * 如此一来，可以针对所有数据库提供程序运行这些测试
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -异步测试清理
  * 删除`Wait`调用，不需要的异步，和重命名某些测试方法
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -统一日志记录测试基础结构
  * 添加`CreateListLoggerFactory`和删除一些以前日志记录基础结构，这将需要使用这些测试能够做出反应的提供程序
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -以异步方式和同步方式运行更多查询测试
  * 测试名称和组成要素已更改，这将需要使用这些测试能够做出反应的提供程序
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -重命名 ComplexNavigations 模型中的导航
  * 提供程序使用这些测试可能需要做出响应
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 -返回到池而不是功能测试中释放上下文
  * 此更改包括这可能需要提供程序以响应某些测试重构


#### <a name="test-and-product-code-changes"></a>测试和产品代码更改

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -合并 RelationalTypeMapping.Clone 方法
  * 允许在派生类中简化 RelationalTypeMapping 到 2.1 中的更改。 我们不相信这中断到提供程序，但提供程序可以充分利用此更改及其派生类型中将类映射。
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -标记或命名查询
  * 添加用于标记 LINQ 查询并显示为 SQL 中的注释这些标记的基础结构。 这可能需要提供程序中的 SQL 生成的做出响应。
* https://github.com/aspnet/EntityFrameworkCore/pull/13115 -支持通过 NTS 空间数据
  * 允许类型映射和成员翻译人员外部提供程序注册
    * 提供程序必须调用基。在其工作其 ITypeMappingSource 实现中 FindMapping()
  * 遵循此模式将空间支持添加到您在提供程序都是一致的提供商。

