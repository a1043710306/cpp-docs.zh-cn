---
title: 用于书签的提供程序支持
ms.date: 11/04/2016
helpviewer_keywords:
- IRowsetLocate class, provider support for bookmarks
- OLE DB provider templates, bookmarks
- bookmarks, OLE DB
- IRowsetLocate class
- OLE DB providers, bookmark support
ms.assetid: 1b14ccff-4f76-462e-96ab-1aada815c377
ms.openlocfilehash: 240cb4da03d6c8c1958b7a86e78171aca2dc30e9
ms.sourcegitcommit: 1f009ab0f2cc4a177f2d1353d5a38f164612bdb1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2020
ms.locfileid: "87216448"
---
# <a name="provider-support-for-bookmarks"></a>用于书签的提供程序支持

本主题中的示例将 `IRowsetLocate` 接口添加到 `CCustomRowset` 类。 几乎在所有情况下，首先将接口添加到现有的 COM 对象。 然后，你可以通过从使用者模板中添加更多调用来对其进行测试。 此示例演示如何执行以下操作：

- 将接口添加到提供程序。

- 动态确定要返回给使用者的列。

- 添加书签支持。

`IRowsetLocate` 接口继承自 `IRowset` 接口。 若要添加 `IRowsetLocate` 接口，请 `CCustomRowset` 从[IRowsetLocateImpl](../../data/oledb/irowsetlocateimpl-class.md)继承。

添加 `IRowsetLocate` 接口与大多数接口有点不同。 为了使 VTABLEs 联机，OLE DB 提供程序模板具有一个用于处理派生接口的模板参数。 下面的代码演示了新的继承列表：

```cpp
////////////////////////////////////////////////////////////////////////
// CustomRS.h

// CCustomRowset
class CCustomRowset : public CRowsetImpl< CCustomRowset,
      CTextData, CCustomCommand, CAtlArray<CTextData>,
      CSimpleRow,
          IRowsetLocateImpl<CCustomRowset, IRowsetLocate>>
```

第四个、第五个和第六个参数都是添加的。 此示例使用第四个和第五个参数的默认值，但指定 `IRowsetLocateImpl` 为第六个参数。 `IRowsetLocateImpl`是一个 OLE DB 模板类，它采用两个模板参数：它们挂钩 `IRowsetLocate` 到类的接口 `CCustomRowset` 。 若要添加大多数接口，可以跳过此步骤并移至下一个。 只 `IRowsetLocate` `IRowsetScroll` 需以这种方式处理和接口。

然后，需要告诉 `CCustomRowset` 调用 `QueryInterface` `IRowsetLocate` 接口。 将行添加 `COM_INTERFACE_ENTRY(IRowsetLocate)` 到地图中。 的接口映射 `CCustomRowset` 应出现，如以下代码所示：

```cpp
////////////////////////////////////////////////////////////////////////
// CustomRS.h

typedef CRowsetImpl< CCustomRowset, CTextData, CCustomCommand, CAtlArray<CTextData>, CSimpleRow, IRowsetLocateImpl<CCustomRowset, IRowsetLocate>> _RowsetBaseClass;

BEGIN_COM_MAP(CCustomRowset)
   COM_INTERFACE_ENTRY(IRowsetLocate)
   COM_INTERFACE_ENTRY_CHAIN(_RowsetBaseClass)
END_COM_MAP()
```

还需要将映射挂钩到 `CRowsetImpl` 类中。 添加到要在映射中挂钩的 COM_INTERFACE_ENTRY_CHAIN 宏中 `CRowsetImpl` 。 此外，还会创建一个名为 `RowsetBaseClass` 的 typedef，其中包含继承信息。 此 typedef 是任意的，可以忽略。

最后，处理 `IColumnsInfo::GetColumnsInfo` 调用。 通常使用 PROVIDER_COLUMN_ENTRY 宏来完成此操作。 但是，使用者可能希望使用书签。 你必须能够更改提供程序返回的列，具体取决于使用者是否请求书签。

若要处理 `IColumnsInfo::GetColumnsInfo` 调用，请在类中删除 PROVIDER_COLUMN 映射 `CTextData` 。 PROVIDER_COLUMN_MAP 宏定义函数 `GetColumnInfo` 。 定义自己的 `GetColumnInfo` 函数。 函数声明应如下所示：

```cpp
////////////////////////////////////////////////////////////////////////
// CustomRS.H

class CTextData
{
   ...
     // NOTE: Be sure you removed the PROVIDER_COLUMN_MAP!
   static ATLCOLUMNINFO* GetColumnInfo(CCustomRowset* pThis,
        ULONG* pcCols);
   static ATLCOLUMNINFO* GetColumnInfo(CCustomCommand* pThis,
        ULONG* pcCols);
...
};
```

然后， `GetColumnInfo` 在*自定义*RS .cpp 文件中实现该函数，如下所示：

```cpp
////////////////////////////////////////////////////////////////////
// CustomRS.cpp

template <class TInterface>
ATLCOLUMNINFO* CommonGetColInfo(IUnknown* pPropsUnk, ULONG* pcCols)
{
   static ATLCOLUMNINFO _rgColumns[5];
   ULONG ulCols = 0;

   CComQIPtr<TInterface> spProps = pPropsUnk;

   CDBPropIDSet set(DBPROPSET_ROWSET);
   set.AddPropertyID(DBPROP_BOOKMARKS);
   DBPROPSET* pPropSet = NULL;
   ULONG ulPropSet = 0;
   HRESULT hr;

   if (spProps)
      hr = spProps->GetProperties(1, &set, &ulPropSet, &pPropSet);

   // Check the property flag for bookmarks, if it is set, set the
// zero ordinal entry in the column map with the bookmark
// information.

   if (pPropSet)
   {
      CComVariant var = pPropSet->rgProperties[0].vValue;
      CoTaskMemFree(pPropSet->rgProperties);
      CoTaskMemFree(pPropSet);

      if ((SUCCEEDED(hr) && (var.boolVal == VARIANT_TRUE)))
      {
         ADD_COLUMN_ENTRY_EX(ulCols, OLESTR("Bookmark"), 0,
                     sizeof(DWORD), DBTYPE_BYTES,
            0, 0, GUID_NULL, CAgentMan, dwBookmark,
                        DBCOLUMNFLAGS_ISBOOKMARK)
         ulCols++;
      }
   }

   // Next set the other columns up.
   ADD_COLUMN_ENTRY_EX(ulCols, OLESTR("Field1"), 1, 16, DBTYPE_STR,
          0xFF, 0xFF, GUID_NULL, CTextData, szField1)
   ulCols++;
   ADD_COLUMN_ENTRY_EX(ulCols, OLESTR("Field2"), 2, 16, DBTYPE_STR,
       0xFF, 0xFF, GUID_NULL, CTextData, szField2)
   ulCols++;

   if (pcCols != NULL)
      *pcCols = ulCols;

   return _rgColumns;
}

ATLCOLUMNINFO* CTextData::GetColumnInfo(CCustomCommand* pThis,
     ULONG* pcCols)
{
   return CommonGetColInfo<ICommandProperties>(pThis->GetUnknown(),
        pcCols);
}

ATLCOLUMNINFO* CAgentMan::GetColumnInfo(RUpdateRowset* pThis, ULONG* pcCols)
{
   return CommonGetColInfo<IRowsetInfo>(pThis->GetUnknown(), pcCols);
}
```

`GetColumnInfo`首先检查是否 `DBPROP_IRowsetLocate` 设置了名为的属性。 OLE DB 包含行集对象的每个可选接口的属性。 如果使用者想要使用这些可选接口中的一个，则将属性设置为 true。 然后，提供程序可以选中此属性，并基于它执行特殊操作。

在您的实现中，您可以使用指向命令对象的指针获取属性。 `pThis`指针表示行集或 command 类。 由于使用的是模板，因此必须在中以指针形式传递此项， **`void`** 否则代码将不会编译。

指定一个静态数组来保存列信息。 如果使用者不需要书签列，则会浪费数组中的条目。 你可以动态分配此数组，但需要确保正确销毁它。 此示例定义并使用宏 ADD_COLUMN_ENTRY 和 ADD_COLUMN_ENTRY_EX 将信息插入到数组中。 可以将宏添加到*自定义*RS。H 文件，如以下代码所示：

```cpp
////////////////////////////////////////////////////////////////////////
// CustomRS.h

#define ADD_COLUMN_ENTRY(ulCols, name, ordinal, colSize, type, precision, scale, guid, dataClass, member) \
   _rgColumns[ulCols].pwszName = (LPOLESTR)name; \
   _rgColumns[ulCols].pTypeInfo = (ITypeInfo*)NULL; \
   _rgColumns[ulCols].iOrdinal = (ULONG)ordinal; \
   _rgColumns[ulCols].dwFlags = 0; \
   _rgColumns[ulCols].ulColumnSize = (ULONG)colSize; \
   _rgColumns[ulCols].wType = (DBTYPE)type; \
   _rgColumns[ulCols].bPrecision = (BYTE)precision; \
   _rgColumns[ulCols].bScale = (BYTE)scale; \
   _rgColumns[ulCols].cbOffset = offsetof(dataClass, member);

#define ADD_COLUMN_ENTRY_EX(ulCols, name, ordinal, colSize, type, precision, scale, guid, dataClass, member, flags) \
   _rgColumns[ulCols].pwszName = (LPOLESTR)name; \
   _rgColumns[ulCols].pTypeInfo = (ITypeInfo*)NULL; \
   _rgColumns[ulCols].iOrdinal = (ULONG)ordinal; \
   _rgColumns[ulCols].dwFlags = flags; \
   _rgColumns[ulCols].ulColumnSize = (ULONG)colSize; \
   _rgColumns[ulCols].wType = (DBTYPE)type; \
   _rgColumns[ulCols].bPrecision = (BYTE)precision; \
   _rgColumns[ulCols].bScale = (BYTE)scale; \
   _rgColumns[ulCols].cbOffset = offsetof(dataClass, member); \
   memset(&(_rgColumns[ulCols].columnid), 0, sizeof(DBID)); \
   _rgColumns[ulCols].columnid.uName.pwszName = (LPOLESTR)name;
```

若要测试使用者中的代码，需要对处理程序进行一些更改 `OnRun` 。 对函数的第一次更改是添加用于向属性集添加属性的代码。 此代码会将 `DBPROP_IRowsetLocate` 属性设置为 true，从而指示您需要书签列的提供程序。 `OnRun`处理程序代码应如下所示：

```cpp
//////////////////////////////////////////////////////////////////////
// TestProv Consumer Application in TestProvDlg.cpp

void CTestProvDlg::OnRun()
{
   CCommand<CAccessor<CProvider>> table;
   CDataSource source;
   CSession   session;

   if (source.Open("Custom.Custom.1", NULL, NULL, NULL,
          NULL) != S_OK)
      return;

   if (session.Open(source) != S_OK)
      return;

   CDBPropSet propset(DBPROPSET_ROWSET);
   propset.AddProperty(DBPROP_IRowsetLocate, true);
   if (table.Open(session, _T("c:\\public\\testprf2\\myData.txt"),
          &propset) != S_OK)
      return;

   CBookmark<4> tempBookmark;
   ULONG ulCount=0;
   while (table.MoveNext() == S_OK)
   {

      DBCOMPARE compare;
      if (ulCount == 2)
         tempBookmark = table.bookmark;

HRESULT hr = table.Compare(table.dwBookmark, table.dwBookmark,
                 &compare);
      if (FAILED(hr))
         ATLTRACE(_T("Compare failed: 0x%X\n"), hr);
      else
         _ASSERTE(compare == DBCOMPARE_EQ);

      m_ctlString1.AddString(table.szField1);
      m_ctlString2.AddString(table.szField2);
      ulCount++;
   }

   table.MoveToBookmark(tempBookmark);
   m_ctlString1.AddString(table.szField1);
   m_ctlString2.AddString(table.szField2);
}
```

**`while`** 循环包含用于调用 `Compare` 接口中的方法的代码 `IRowsetLocate` 。 由于要比较完全相同的书签，因此应始终传递您的代码。 此外，在临时变量中存储一个书签，以便在循环完成后可以使用它 **`while`** 来调用 `MoveToBookmark` 使用者模板中的函数。 `MoveToBookmark`函数调用中的 `GetRowsAt` 方法 `IRowsetLocate` 。

还需要更新使用者中的用户记录。 在类中添加一个条目以处理书签和 COLUMN_MAP 中的条目：

```cpp
///////////////////////////////////////////////////////////////////////
// TestProvDlg.cpp

class CProvider
{
// Attributes
public:
   CBookmark<4>    bookmark;  // Add this line
   char   szCommand[16];
   char   szText[256];

   // Binding Maps
BEGIN_ACCESSOR_MAP(CProvider, 1)
   BEGIN_ACCESSOR(0, true)   // auto accessor
      BOOKMARK_ENTRY(bookmark)  // Add this line
      COLUMN_ENTRY(1, szField1)
      COLUMN_ENTRY(2, szField2)
   END_ACCESSOR()
END_ACCESSOR_MAP()
};
```

更新代码后，应该能够用接口生成并执行提供程序 `IRowsetLocate` 。

## <a name="see-also"></a>另请参阅

[高级提供程序技术](../../data/oledb/advanced-provider-techniques.md)
