---
title: __uncaught_exception
ms.date: 11/04/2016
api_name:
- __uncaught_exception
api_location:
- msvcrt.dll
- msvcr80.dll
- msvcr90.dll
- msvcr100.dll
- msvcr100_clr0400.dll
- msvcr110.dll
- msvcr110_clr0400.dll
- msvcr120.dll
- msvcr120_clr0400.dll
- ucrtbase.dll
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- __uncaught_exception
helpviewer_keywords:
- __uncaught_exception
ms.assetid: 4d9b75c6-c9c7-4876-b761-ea9ab1925e96
ms.openlocfilehash: 1eb06abbda7978acf578555f966f0857dff02053
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2020
ms.locfileid: "87211627"
---
# <a name="__uncaught_exception"></a>__uncaught_exception

指示是否已引发一个或多个异常，但尚未由 **`catch`** [try-catch](../../cpp/try-throw-and-catch-statements-cpp.md)语句的相应块处理。

## <a name="syntax"></a>语法

```cpp
bool __uncaught_exception(
   );
```

## <a name="return-value"></a>返回值

**`true`** 在块中引发异常时， **`try`** 直到匹配 **`catch`** 块初始化; 否则为 **`false`** 。

## <a name="remarks"></a>备注

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|
|-------------|---------------------|
|__uncaught_exception|eh.h|

## <a name="see-also"></a>另请参阅

[try、throw 和 catch 语句 (C++)](../../cpp/try-throw-and-catch-statements-cpp.md)<br/>
