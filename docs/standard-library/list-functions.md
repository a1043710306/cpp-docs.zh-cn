---
title: '&lt;list&gt; 函数 |Microsoft Docs'
ms.custom: ''
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- list/std::swap
ms.openlocfilehash: 04f00a9274018432cd03917ae5485f2d395649e4
ms.sourcegitcommit: 7ecd91d8ce18088a956917cdaf3a3565bd128510
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2020
ms.locfileid: "79425603"
---
# <a name="ltlistgt-functions"></a>&lt;列表&gt; 函数

## <a name="swap"></a>购

交换两个列表的元素。

```cpp
template <class T, class Allocator>
    void swap(list<T, Allocator>& left, list<T, Allocator>& right)
```

### <a name="parameters"></a>parameters

*左*\
一个 `list` 类型的对象。

*right*\
一个 `list` 类型的对象。

### <a name="remarks"></a>备注

该模板函数执行 `left.swap(right)`。
