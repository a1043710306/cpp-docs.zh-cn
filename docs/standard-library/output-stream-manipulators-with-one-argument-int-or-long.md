---
title: "带有一个自变量（int 或 long）的输出流操控器 | Microsoft Docs"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology:
- cpp-standard-libraries
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- C++
helpviewer_keywords:
- output streams, int or long argument manipulators
ms.assetid: 338f3164-b5e2-4c5a-a605-7d9dc3629ca1
caps.latest.revision: 8
author: corob-msft
ms.author: corob
manager: ghogen
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Machine Translation
ms.sourcegitcommit: 3168772cbb7e8127523bc2fc2da5cc9b4f59beb8
ms.openlocfilehash: 84cbc5d016f6796a1cab208a1d77b51ea2ac6ecb
ms.contentlocale: zh-cn
ms.lasthandoff: 02/24/2017

---
# <a name="output-stream-manipulators-with-one-argument-int-or-long"></a>带有一个自变量（int 或 long）的输出流操控器
iostream 类库提供用于创建参数化操控器的一组宏。 具有单个 `int` 或 `long` 参数的操控器是一种特殊情况。 若要创建接受单个 `int` 或 `long` 参数（如 `setw`）的输出流操控器，则必须使用在 \<iomanip> 中定义的 _Smanip 宏。 此示例定义了向流中插入指定数量的空格的 `fillblank` 操控器：  
  
## <a name="example"></a>示例  
  
```cpp  
// output_stream_manip.cpp  
// compile with: /GR /EHsc  
#include <iostream>  
#include <iomanip>  
using namespace std;  
  
void fb( ios_base& os, int l )  
{  
   ostream *pos = dynamic_cast<ostream*>(&os);  
   if (pos)  
   {  
      for( int i=0; i < l; i++ )  
      (*pos) << ' ';  
   };  
}  
  
_Smanip<int>  
   __cdecl fillblank(int no)  
   {     
   return (_Smanip<int>(&fb, no));  
   }  
  
int main( )  
{  
   cout << "10 blanks follow" << fillblank( 10 ) << ".\n";  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [带自变量的自定义操控器](../standard-library/custom-manipulators-with-arguments.md)

