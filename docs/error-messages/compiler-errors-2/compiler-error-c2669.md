---
title: 编译器错误 C2669
ms.date: 11/04/2016
f1_keywords:
- C2669
helpviewer_keywords:
- C2669
ms.assetid: f9cb8111-bcdc-484b-a863-2c42e15a0496
ms.openlocfilehash: 7944ced947ba1d7c8b10172560ce6237a554e236
ms.sourcegitcommit: 0ab61bc3d2b6cfbd52a16c6ab2b97a8ea1864f12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62300786"
---
# <a name="compiler-error-c2669"></a>编译器错误 C2669

不允许匿名联合中的成员函数

[匿名联合](../../cpp/unions.md#anonymous_unions)不能有成员函数。

## <a name="example"></a>示例

下面的示例生成 C2669:

```cpp
// C2669.cpp
struct X {
   union {
      int i;
      void f() {   // C2669, remove function
         i = 0;
      }
   };
};
```
