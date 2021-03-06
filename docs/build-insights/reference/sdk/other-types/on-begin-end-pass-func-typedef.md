---
title: OnBeginEndPassFunc typedef
description: The C++ Build Insights SDK OnBeginEndPassFunc typedef 引用。
ms.date: 02/12/2020
helpviewer_keywords:
- C++ Build Insights
- C++ Build Insights SDK
- OnBeginEndPassFunc
- throughput analysis
- build time analysis
- vcperf.exe
ms.openlocfilehash: 2008dfb86d6f45a1c05a59e1f0f4f8c7868dcda2
ms.sourcegitcommit: 6280a4c629de0f638ebc2edd446de2a9b11f0406
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2020
ms.locfileid: "90041973"
---
# <a name="onbeginendpassfunc-typedef"></a>OnBeginEndPassFunc typedef

::: moniker range="<=vs-2015"

C++ Build Insights SDK 与 Visual Studio 2017 及更高版本兼容。 若要查看这些版本的文档，请将本文的 Visual Studio“版本”选择器控件设置为 Visual Studio 2017 或 Visual Studio 2019。 它位于此页面上目录表的顶部。

::: moniker-end
::: moniker range=">=vs-2017"

`OnBeginEndPassFunc` typedef 是在 [ANALYSIS_CALLBACKS](analysis-callbacks-struct.md) 和 [RELOG_CALLBACKS](relog-callbacks-struct.md) 结构中使用的函数签名之一。

## <a name="syntax"></a>语法

```cpp
typedef enum CALLBACK_CODE (BUILD_INSIGHTS_API *OnBeginEndPassFunc)(
    void*                           callbackContext);
```

## <a name="members"></a>成员

| 名称 | 说明 |
|--|--|
| `callbackContext` |  |

::: moniker-end
