---
title: '&lt;string&gt; typedef'
ms.date: 11/04/2016
f1_keywords:
- string/std::string
- string/std::u16string
- string/std::u32string
- string/std::wstring
ms.assetid: fdca01e9-f2f1-4b59-abda-0093d760b3cc
ms.openlocfilehash: 083209f0121ac38d8adf81975577257e4e23a393
ms.sourcegitcommit: ec6dd97ef3d10b44e0fedaa8e53f41696f49ac7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88845427"
---
# <a name="ltstringgt-typedefs"></a>&lt;string&gt; typedef

[类似](#string)\
[u16string](#u16string)\
[u32string](#u32string)\
[wstring](#wstring)

## <a name="string"></a><a name="string"></a> 字符串

一种类型，该类型描述与类型的元素 [basic_string](../standard-library/basic-string-class.md) 的类模板的专用化 **`char`** 。

专用化 `basic_string` 的其他 typedef 包括 [wstring](../standard-library/string-typedefs.md#wstring)、[u16string](../standard-library/string-typedefs.md#u16string) 和 [u32string](../standard-library/string-typedefs.md#u32string)。

```cpp
typedef basic_string<char, char_traits<char>, allocator<char>> string;
```

### <a name="remarks"></a>注解

以下是等效声明：

```cpp
string str("");

basic_string<char> str("");
```

关于字符串构造函数的列表，请参阅 [basic_string:: basic_string](../standard-library/basic-string-class.md#basic_string)。

## <a name="u16string"></a><a name="u16string"></a> u16string

一种类型，该类型描述与类型的元素 [basic_string](../standard-library/basic-string-class.md) 的类模板的专用化 **`char16_t`** 。

专用化 `basic_string` 的其他 typedef 包括 [wstring](../standard-library/string-typedefs.md#wstring)、[string](../standard-library/string-typedefs.md#string) 和 [u32string](../standard-library/string-typedefs.md#u32string)。

```cpp
typedef basic_string<char16_t, char_traits<char16_t>, allocator<char16_t>> u16string;
```

### <a name="remarks"></a>注解

关于字符串构造函数的列表，请参阅 [basic_string:: basic_string](../standard-library/basic-string-class.md#basic_string)。

## <a name="u32string"></a><a name="u32string"></a> u32string

一种类型，该类型描述与类型的元素 [basic_string](../standard-library/basic-string-class.md) 的类模板的专用化 **`char32_t`** 。

专用化 `basic_string` 的其他 typedef 包括 [tring](../standard-library/string-typedefs.md#string)、[u16string](../standard-library/string-typedefs.md#u16string) 和 [wstring](../standard-library/string-typedefs.md#wstring)。

```cpp
typedef basic_string<char32_t, char_traits<char32_t>, allocator<char32_t>> u32string;
```

### <a name="remarks"></a>注解

关于字符串构造函数的列表，请参阅 [basic_string:: basic_string](../standard-library/basic-string-class.md#basic_string)。

## <a name="wstring"></a><a name="wstring"></a> wstring

一种类型，该类型描述与类型的元素 [basic_string](../standard-library/basic-string-class.md) 的类模板的专用化 **`wchar_t`** 。

专用化 `basic_string` 的其他 typedef 包括 [string](../standard-library/string-typedefs.md#string)、[u16string](../standard-library/string-typedefs.md#u16string) 和 [u32string](../standard-library/string-typedefs.md#u32string)。

```cpp
typedef basic_string<wchar_t, char_traits<wchar_t>, allocator<wchar_t>> wstring;
```

### <a name="remarks"></a>注解

以下是等效声明：

```cpp
wstring wstr(L"");

basic_string<wchar_t> wstr(L"");
```

关于字符串构造函数的列表，请参阅 [basic_string:: basic_string](../standard-library/basic-string-class.md#basic_string)。

> [!NOTE]
> 的大小 **`wchar_t`** 是由实现定义的。 如果代码依赖于 **`wchar_t`** 特定大小，请检查平台的实现 (例如，使用 `sizeof(wchar_t)`) 。 如果需要保证宽度在所有平台上一致的字符串字符类型，请使用 [string](../standard-library/string-typedefs.md#string)、[u16string](../standard-library/string-typedefs.md#u16string) 或 [u32string](../standard-library/string-typedefs.md#u32string)。

## <a name="see-also"></a>另请参阅

[\<string>](../standard-library/string.md)
