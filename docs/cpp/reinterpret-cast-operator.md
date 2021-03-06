---
title: reinterpret_cast 运算符
ms.date: 11/04/2016
f1_keywords:
- reinterpret_cast_cpp
helpviewer_keywords:
- reinterpret_cast keyword [C++]
ms.assetid: eb3283c7-7f88-467e-affd-407d37b46d6c
ms.openlocfilehash: 33da7427adeb0a0cade2a369664d7fbd34790681
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2020
ms.locfileid: "87233595"
---
# <a name="reinterpret_cast-operator"></a>reinterpret_cast 运算符

允许将任何指针转换为任何其他指针类型。 也允许将任何整数类型转换为任何指针类型以及反向转换。

## <a name="syntax"></a>语法

```
reinterpret_cast < type-id > ( expression )
```

## <a name="remarks"></a>备注

滥用操作员的 **`reinterpret_cast`** 操作很容易就会不安全。 除非所需转换本身是低级别的，否则应使用其他强制转换运算符之一。

**`reinterpret_cast`** 运算符可用于转换（例如）或转换 **`char*`** **`int*`** `One_class*` 为 `Unrelated_class*` ，它们本质上是不安全的。

**`reinterpret_cast`** 除强制转换回其原始类型外，不能安全地使用的结果。 在最好的情况下，其他用途也是不可移植的。

**`reinterpret_cast`** 运算符不能转换为 **`const`** 、 **`volatile`** 或 **`__unaligned`** 特性。 有关删除这些属性的信息，请参阅[Const_cast 运算符](../cpp/const-cast-operator.md)。

**`reinterpret_cast`** 运算符将 null 指针值转换为目标类型的 null 指针值。

的一种实际用法 **`reinterpret_cast`** 是在哈希函数中，将值映射到索引，这种方式的两个不同值很少会以相同的索引结束。

```cpp
#include <iostream>
using namespace std;

// Returns a hash code based on an address
unsigned short Hash( void *p ) {
   unsigned int val = reinterpret_cast<unsigned int>( p );
   return ( unsigned short )( val ^ (val >> 16));
}

using namespace std;
int main() {
   int a[20];
   for ( int i = 0; i < 20; i++ )
      cout << Hash( a + i ) << endl;
}

Output:
64641
64645
64889
64893
64881
64885
64873
64877
64865
64869
64857
64861
64849
64853
64841
64845
64833
64837
64825
64829
```

**`reinterpret_cast`** 允许将指针视为整型。 结果随后将按位移位并与自身进行“异或”运算以生成唯一的索引（具有唯一性的概率非常高）。 该索引随后被标准 C 样式强制转换截断为函数的返回类型。

## <a name="see-also"></a>另请参阅

[转换运算符](../cpp/casting-operators.md)<br/>
[关键字](../cpp/keywords-cpp.md)
