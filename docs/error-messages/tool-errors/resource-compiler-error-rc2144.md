---
title: 资源编译器错误 RC2144
ms.date: 11/04/2016
f1_keywords:
- RC2144
helpviewer_keywords:
- RC2144
ms.assetid: 1b3ff39a-92cd-4a04-b1a3-b1fa6a805813
ms.openlocfilehash: 1b080916642fc1be4b22820668c4cb4137675425
ms.sourcegitcommit: 857fa6b530224fa6c18675138043aba9aa0619fb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80191192"
---
# <a name="resource-compiler-error-rc2144"></a>资源编译器错误 RC2144

主语言 ID 不是数字

主语言 ID 必须是十六进制语言 ID。 有关有效的语言 ID 的列表，请参阅 [《运行时库参考》](../../c-runtime-library/locale-names-languages-and-country-region-strings.md) 中的 *语言和国家/地区字符串* 。

如果已使用工具添加资源并将其从 .RC 文件中删除，也可能发生此错误。 若要解决此问题，请在文本编辑器中打开 .RC 文件并手动清理任何未使用的资源。
