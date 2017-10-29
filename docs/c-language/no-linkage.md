---
title: "无链接 | Microsoft Docs"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology:
- cpp-language
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- C++
helpviewer_keywords:
- no linkage
- linkage [C++], none
ms.assetid: 5a413082-1034-4e04-b76b-8d14668bf434
caps.latest.revision: 6
author: mikeblome
ms.author: mblome
manager: ghogen
ms.translationtype: HT
ms.sourcegitcommit: 16d1bf59dfd4b3ef5f037aed9c0f6febfdf1a2e8
ms.openlocfilehash: 278325c1ad1b31ce20b6a17034be5e4731f6da78
ms.contentlocale: zh-cn
ms.lasthandoff: 10/09/2017

---
# <a name="no-linkage"></a>无链接
如果块内的某个标识符的声明不包括 `extern` 存储类说明符，则该标识符没有链接并且对函数是唯一的。  
  
 以下标识符没有链接：  
  
-   声明为除对象或函数以外的任何项的标识符  
  
-   声明为函数参数的标识符  
  
-   声明时未使用 `extern` 存储类说明符的对象的块范围标识符  
  
 如果标识符没有链接，那么在相同的范围级别内再次声明相同的名称（在声明符或类型说明符中）将产生符号重新定义错误。  
  
## <a name="see-also"></a>另请参阅  
 [使用 extern 指定链接](../cpp/using-extern-to-specify-linkage.md)