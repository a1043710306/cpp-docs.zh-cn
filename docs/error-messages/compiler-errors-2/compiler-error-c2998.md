---
title: "编译器错误 C2998 |Microsoft 文档"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology:
- cpp-tools
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- C2998
dev_langs:
- C++
helpviewer_keywords:
- C2998
ms.assetid: 8193d491-b5d9-4477-acb1-cf166889c070
caps.latest.revision: 7
author: corob-msft
ms.author: corob
manager: ghogen
ms.translationtype: MT
ms.sourcegitcommit: 35b46e23aeb5f4dbfd2a0dd44b906389dd5bfc88
ms.openlocfilehash: 64a1bf4d496e31f5d189b315180a6bd60d4bc867
ms.contentlocale: zh-cn
ms.lasthandoff: 10/10/2017

---
# <a name="compiler-error-c2998"></a>编译器错误 C2998
“identifier”：不能是模板定义  
  
 编译器无法处理模板定义中使用的语法。  
  
 下面的示例生成 C2998：  
  
```  
// C2998.cpp  
// compile with: /c  
template <class T> int x = 1018; // C2998  
```