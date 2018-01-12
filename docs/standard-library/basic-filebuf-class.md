---
title: "basic_filebuf 类 | Microsoft 文档"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology: cpp-standard-libraries
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- fstream/std::basic_filebuf
- fstream/std::basic_filebuf::char_type
- fstream/std::basic_filebuf::int_type
- fstream/std::basic_filebuf::off_type
- fstream/std::basic_filebuf::pos_type
- fstream/std::basic_filebuf::traits_type
- fstream/std::basic_filebuf::close
- fstream/std::basic_filebuf::is_open
- fstream/std::basic_filebuf::open
- fstream/std::basic_filebuf::overflow
- fstream/std::basic_filebuf::pbackfail
- fstream/std::basic_filebuf::seekoff
- fstream/std::basic_filebuf::seekpos
- fstream/std::basic_filebuf::setbuf
- fstream/std::basic_filebuf::Swap
- fstream/std::basic_filebuf::sync
- fstream/std::basic_filebuf::uflow
- fstream/std::basic_filebuf::underflow
dev_langs: C++
helpviewer_keywords:
- std::basic_filebuf [C++]
- std::basic_filebuf [C++], char_type
- std::basic_filebuf [C++], int_type
- std::basic_filebuf [C++], off_type
- std::basic_filebuf [C++], pos_type
- std::basic_filebuf [C++], traits_type
- std::basic_filebuf [C++], close
- std::basic_filebuf [C++], is_open
- std::basic_filebuf [C++], open
- std::basic_filebuf [C++], overflow
- std::basic_filebuf [C++], pbackfail
- std::basic_filebuf [C++], seekoff
- std::basic_filebuf [C++], seekpos
- std::basic_filebuf [C++], setbuf
- std::basic_filebuf [C++], Swap
- std::basic_filebuf [C++], sync
- std::basic_filebuf [C++], uflow
- std::basic_filebuf [C++], underflow
ms.assetid: 3196ba5c-bf38-41bd-9a95-70323ddfca1a
caps.latest.revision: "24"
author: corob-msft
ms.author: corob
manager: ghogen
ms.workload: cplusplus
ms.openlocfilehash: 0c5ec1881695c80c8f493ac2a2848d0349f430aa
ms.sourcegitcommit: 8fa8fdf0fbb4f57950f1e8f4f9b81b4d39ec7d7a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2017
---
# <a name="basicfilebuf-class"></a>basic_filebuf 类
描述对 `Elem` 类型的元素（其字符特征由类 `Tr` 确定）与外部文件中存储的元素序列之间的来回传输进行控制的流缓冲区。  
  
## <a name="syntax"></a>语法  
  
```  
template <class Elem, class Tr = char_traits<Elem>>  
class basic_filebuf : public basic_streambuf<Elem, Tr>  
```  
  
#### <a name="parameters"></a>参数  
 `Elem`  
 文件缓冲区的基本元素。  
  
 `Tr`  
 文件缓冲区的基本元素的特征（通常是 `char_traits`< `Elem`>）。  
  
## <a name="remarks"></a>备注  
 该模板类描述对 `Elem` 类型的元素（其字符特征由类 `Tr` 确定）与外部文件中存储的元素序列之间的来回传输进行控制的流缓冲区。  
  
> [!NOTE]
>  `basic_filebuf` 类型的对象使用 `char *` 类型的内部缓冲区创建（不考虑由类型参数 `Elem` 指定的 `char_type`）。 这意味着，Unicode 字符串（包含 `wchar_t` 字符）会在写入内部缓冲区之前转换为 ANSI 字符串（包含 `char` 字符）。 若要在缓冲区中存储 Unicode 字符串，请创建 `wchar_t` 类型的新缓冲区，并使用 [basic_streambuf::pubsetbuf](../standard-library/basic-streambuf-class.md#pubsetbuf)`()` 方法设置它。 若要查看演示此行为的示例，请参阅下面的内容。  
  
 类 `basic_filebuf`< `Elem`, `Tr`> 的对象会存储文件指针，它指定的 `FILE` 对象控制与打开的文件关联的流。 另外，它还存储指向两个文件转换方面的指针，以供受保护的成员函数 [overflow](#overflow) 和 [underflow](#underflow) 使用。 有关详细信息，请参阅 [basic_filebuf::open](#open)。  
  
## <a name="example"></a>示例  
 下面的示例演示如何通过调用 `pubsetbuf()` 方法来强制 `basic_filebuf<wchar_t>` 类型的对象将 Unicode 字符存储在其内部缓冲区中。  
  
```  
// unicode_basic_filebuf.cpp  
// compile with: /EHsc  
  
#include <iostream>  
#include <string>  
#include <fstream>  
#include <iomanip>  
#include <memory.h>  
#include <string.h>  
  
#define IBUFSIZE 16  
  
using namespace std;  
  
void hexdump(const string& filename);  
  
int main()  
{  
    wchar_t* wszHello = L"Hello World";  
    wchar_t wBuffer[128];  
  
    basic_filebuf<wchar_t> wOutFile;  
  
    // Open a file, wcHello.txt, then write to it, then dump the  
    // file's contents in hex  
    wOutFile.open("wcHello.txt",  
        ios_base::out | ios_base::trunc | ios_base::binary);  
    if(!wOutFile.is_open())  
    {  
        cout << "Error Opening wcHello.txt\n";  
        return -1;  
    }  
    wOutFile.sputn(wszHello, (streamsize)wcslen(wszHello));  
    wOutFile.close();  
    cout << "Hex Dump of wcHello.txt - note that output is ANSI chars:\n";  
    hexdump(string("wcHello.txt"));  
  
    // Open a file, wwHello.txt, then set the internal buffer of  
    // the basic_filebuf object to be of type wchar_t, then write  
    // to the file and dump the file's contents in hex  
    wOutFile.open("wwHello.txt",  
        ios_base::out | ios_base::trunc | ios_base::binary);  
    if(!wOutFile.is_open())  
    {  
        cout << "Error Opening wwHello.txt\n";  
        return -1;  
    }  
    wOutFile.pubsetbuf(wBuffer, (streamsize)128);  
    wOutFile.sputn(wszHello, (streamsize)wcslen(wszHello));  
    wOutFile.close();  
    cout << "\nHex Dump of wwHello.txt - note that output is wchar_t chars:\n";  
    hexdump(string("wwHello.txt"));  
  
    return 0;  
}  
  
// dump contents of filename to stdout in hex  
void hexdump(const string& filename)  
{  
    fstream ifile(filename.c_str(),  
        ios_base::in | ios_base::binary);  
    char *ibuff = new char[IBUFSIZE];  
    char *obuff = new char[(IBUFSIZE*2)+1];  
    int i;  
  
    if(!ifile.is_open())  
    {  
        cout << "Cannot Open " << filename.c_str()  
             << " for reading\n";  
        return;  
    }  
    if(!ibuff || !obuff)  
    {  
        cout << "Cannot Allocate buffers\n";  
        ifile.close();  
        return;  
    }  
  
    while(!ifile.eof())  
    {  
        memset(obuff,0,(IBUFSIZE*2)+1);  
        memset(ibuff,0,IBUFSIZE);  
        ifile.read(ibuff,IBUFSIZE);  
  
        // corner case where file is exactly a multiple of  
        // 16 bytes in length  
        if(ibuff[0] == 0 && ifile.eof())  
            break;  
  
        for(i = 0; i < IBUFSIZE; i++)  
        {  
            if(ibuff[i] >= ' ')  
                obuff[i] = ibuff[i];  
            else  
                obuff[i] = '.';  
  
            cout << setfill('0') << setw(2) << hex  
                 << (int)ibuff[i] << ' ';  
        }  
        cout << "  " << obuff << endl;  
    }  
    ifile.close();  
}  
```  
  
```Output  
Hex Dump of wcHello.txt - note that output is ANSI chars:  
48 65 6c 6c 6f 20 57 6f 72 6c 64 00 00 00 00 00   Hello World.....  
  
Hex Dump of wwHello.txt - note that output is wchar_t chars:  
48 00 65 00 6c 00 6c 00 6f 00 20 00 57 00 6f 00   H.e.l.l.o. .W.o.  
72 00 6c 00 64 00 00 00 00 00 00 00 00 00 00 00   r.l.d...........  
```  
  
### <a name="constructors"></a>构造函数  
  
|||  
|-|-|  
|[basic_filebuf](#basic_filebuf)|构造 `basic_filebuf` 类型的对象。|  
  
### <a name="typedefs"></a>Typedef  
  
|||  
|-|-|  
|[char_type](#char_type)|将类型名与 `Elem` 模板参数关联。|  
|[int_type](#int_type)|使 `basic_filebuf` 范围中的此类型等效于 `Tr` 范围中具有相同名称的类型。|  
|[off_type](#off_type)|使 `basic_filebuf` 范围中的此类型等效于 `Tr` 范围中具有相同名称的类型。|  
|[pos_type](#pos_type)|使 `basic_filebuf` 范围中的此类型等效于 `Tr` 范围中具有相同名称的类型。|  
|[traits_type](#traits_type)|将类型名与 `Tr` 模板参数关联。|  
  
### <a name="member-functions"></a>成员函数  
  
|||  
|-|-|  
|[close](#close)|关闭文件。|  
|[is_open](#is_open)|指示文件是否打开。|  
|[open](#open)|打开文件。|  
|[overflow](#overflow)|将新字符插入到已满缓冲区时可以调用的受保护虚函数。|  
|[pbackfail](#pbackfail)|受保护虚拟成员函数尝试将元素放回到输入流中，随后使它成为当前元素（由下一个指针指向）。|  
|[seekoff](#seekoff)|受保护虚拟成员函数尝试更改受控制流的当前位置。|  
|[seekpos](#seekpos)|受保护虚拟成员函数尝试更改受控制流的当前位置。|  
|[setbuf](#setbuf)|受保护虚拟成员函数执行特定于每个派生流缓冲区的操作。|  
|[Swap](#swap)|为提供的 `basic_filebuf` 参数的内容交换此 `basic_filebuf` 的内容。|  
|[sync](#sync)|受保护虚函数尝试将受控制流与任何关联的外部流同步。|  
|[uflow](../standard-library/basic-streambuf-class.md#uflow)|受保护虚函数从输入流中提取当前元素。|  
|[underflow](#underflow)|受保护虚函数从输入流中提取当前元素。|  
  
## <a name="requirements"></a>惠?  
 **标头：**\<fstream>  
  
 **命名空间：** std  
  
##  <a name="basic_filebuf"></a>  basic_filebuf::basic_filebuf  
 构造 `basic_filebuf` 类型的对象。  
  
```  
basic_filebuf();

basic_filebuf(basic_filebuf&& right);
```  
  
### <a name="remarks"></a>备注  
 第一个构造函数将 null 指针存储在控制输入缓冲区和输出缓冲区的所有指针中。 同时它还将 null 指针存储在文件指针中。  
  
 第二个构造函数初始化具有 `right` 的内容的对象，将其视为右值引用。  
  
##  <a name="char_type"></a>  basic_filebuf::char_type  
 将类型名与 **Elem** 模板参数关联。  
  
```  
typedef Elem char_type;  
```  
  
##  <a name="close"></a>  basic_filebuf::close  
 关闭文件。  
  
```  
basic_filebuf<Elem, Tr> *close();
```  
  
### <a name="return-value"></a>返回值  
 如果文件指针为 null 指针，则该成员函数返回 null 指针。  
  
### <a name="remarks"></a>备注  
 **close** calls `fclose`( **fp**)。 如果该函数返回一个非零值，则该函数返回 null 指针。 否则，它将返回 **this** 以指示已成功关闭文件。  
  
 对于宽流，如果自打开流后，或者自上次调用 `streampos` 后发生任何插入，该函数都将调用 [overflow](#overflow)。 它还将根据需要通过使用文件转换方面 **fac** 调用 **fac.unshift**，插入任何所需的序列，以还原初始转换状态。 因此，生成的类型 `char` 的每个元素 **byte** 将写入文件指针 **fp** 指定的相关流，就像连续调用窗体 `fputc`( **byte**, **fp**) 一样。 如果调用 **fac.unshift** 或任何写入失败，该函数将不会成功。  
  
### <a name="example"></a>示例  
  下面的示例假设当前目录中的两个文件： basic_filebuf_close.txt （内容为 "testing"）和 iotest.txt（内容为 "ssss"）。  
  
```  
// basic_filebuf_close.cpp  
// compile with: /EHsc  
#include <fstream>  
#include <iostream>  
  
int main() {  
   using namespace std;  
   ifstream file;  
   basic_ifstream <wchar_t> wfile;  
   char c;  
   // Open and close with a basic_filebuf  
   file.rdbuf()->open( "basic_filebuf_close.txt", ios::in );  
   file >> c;  
   cout << c << endl;  
   file.rdbuf( )->close( );  
  
   // Open/close directly  
   file.open( "iotest.txt" );  
   file >> c;  
   cout << c << endl;  
   file.close( );  
  
   // open a file with a wide character name  
   wfile.open( L"iotest.txt" );  
  
   // Open and close a nonexistent with a basic_filebuf  
   file.rdbuf()->open( "ziotest.txt", ios::in );  
   cout << file.fail() << endl;  
   file.rdbuf( )->close( );  
  
   // Open/close directly  
   file.open( "ziotest.txt" );  
   cout << file.fail() << endl;  
   file.close( );  
}  
```  
  
```Output  
t  
s  
0  
1  
```  
  
##  <a name="int_type"></a>  basic_filebuf::int_type  
 使 basic_filebuf 范围中的此类型等效于 **Tr** 范围中具有相同名称的类型。  
  
```  
typedef typename traits_type::int_type int_type;  
```  
  
##  <a name="is_open"></a>  basic_filebuf::is_open  
 指示文件是否打开。  
  
```  
bool is_open() const;
```  
  
### <a name="return-value"></a>返回值  
 如果文件指针不是 null 指针，则返回 **true**。  
  
### <a name="example"></a>示例  
  
```cpp  
// basic_filebuf_is_open.cpp  
// compile with: /EHsc  
#include <fstream>  
#include <iostream>  
  
int main( )   
{  
   using namespace std;  
   ifstream file;  
   cout << boolalpha << file.rdbuf( )->is_open( ) << endl;  
  
   file.open( "basic_filebuf_is_open.cpp" );  
   cout << file.rdbuf( )->is_open( ) << endl;  
}  
```  
  
```Output  
false  
true  
```  
  
##  <a name="off_type"></a>  basic_filebuf::off_type  
 使 basic_filebuf 范围中的此类型等效于 **Tr** 范围中具有相同名称的类型。  
  
```  
typedef typename traits_type::off_type off_type;  
```  
  
##  <a name="open"></a>  basic_filebuf::open  
 打开文件。  
  
```  
basic_filebuf<Elem, Tr> *open(
    const char* _Filename,  
    ios_base::openmode _Mode,  
    int _Prot = (int)ios_base::_Openprot);

basic_filebuf<Elem, Tr> *open(
    const char* _Filename,  
    ios_base::openmode _Mode);

basic_filebuf<Elem, Tr> *open(
    const wchar_t* _Filename,  
    ios_base::openmode _Mode,  
    int _Prot = (int)ios_base::_Openprot);

basic_filebuf<Elem, Tr> *open(
    const wchar_t* _Filename,  
    ios_base::openmode _Mode);
```  
  
### <a name="parameters"></a>参数  
 `_Filename`  
 要打开的文件的名称。  
  
 `_Mode`  
 [ios_base::openmode](../standard-library/ios-base-class.md#openmode) 中的枚举之一。  
  
 `_Prot`  
 默认文件打开保护，等同于 [_fsopen、_wfsopen](../c-runtime-library/reference/fsopen-wfsopen.md) 中的 `shflag` 参数。  
  
### <a name="return-value"></a>返回值  
 如果文件指针为 null 指针，则该函数返回 null 指针。 否则，返回 **this**。  
  
### <a name="remarks"></a>备注  
 成员函数通过调用 [fopen](../c-runtime-library/reference/fopen-wfopen.md)( *filename*, **strmode**)，打开文件名为 *filename* 的文件。 **strmode** 通过 **mode &**~( [ate](../standard-library/ios-base-class.md#openmode) & &#124; [binary](../standard-library/ios-base-class.md#openmode)) 确定：  
  
- **ios_base::in** 变为 **"r"**（打开现有文件以进行读取）。  
  
- [ios_base::out](../standard-library/ios-base-class.md#fmtflags) 或 **ios_base::out &#124; ios_base::trunc** 变为 **"w"**（截断现有文件或者创建以进行写入）。  
  
- **ios_base::out &#124; app** 变为 **"a"**（打开现有文件以追加所有写入）。  
  
- **ios_base::in &#124; ios_base::out** 变为 **"r+"**（打开现有文件以读取和写入）。  
  
- **ios_base::in &#124; ios_base::out &#124; ios_base::trunc** 变为 **"w"**（截断现有文件或者创建以读取和写入）。  
  
- **ios_base::in &#124; ios_base::out &#124; ios_base::app** 变为 **"a+"**（打开现有文件以读取和追加所有写入）。  
  
 如果 **mode & ios_base::binary** 为非零值，该函数将追加 **b** 到 **strmode**，以打开一个二进制流而不是文本流。 然后，它将存储文件指针 **fp** 中的 `fopen` 返回的值。 如果 **mode & ios_base::ate** 为非零值，并且文件指针不是 null 指针，则函数调用 `fseek`( **fp**, 0, `SEEK_END`) 将流定位至文件的末尾。 如果该定位操作将失败，函数调用 [close](#close)( **fp**)，并将 null 指针存储在文件指针中。  
  
 如果文件指针不是 null 指针，此函数将确定文件转换方面：`use_facet`< `codecvt`< **Elem**, `char`, **traits_type::**[state_type](../standard-library/char-traits-struct.md#state_type)> >( [getloc](../standard-library/basic-streambuf-class.md#getloc))，以供 [underflow](#underflow) 和 [overflow](#overflow) 使用。  
  
 如果文件指针为 null 指针，则该函数返回 null 指针。 否则，返回 **this**。  
  
### <a name="example"></a>示例  
  有关如何使用 **open** 的示例，请参阅 [basic_filebuf::close](#close)。  
  
##  <a name="op_eq"></a>  basic_filebuf::operator=  
 分配此流缓冲区对象的内容。 这是一种移动赋值，所涉右值不会留下副本。  
  
```  
basic_filebuf& operator=(basic_filebuf&& right);
```  
  
### <a name="parameters"></a>参数  
 `right`  
 对 [basic_filebuf](../standard-library/basic-filebuf-class.md) 对象的右值引用。  
  
### <a name="return-value"></a>返回值  
 返回 *this。  
  
### <a name="remarks"></a>备注  
 成员运算符使用 `right` 内容替换该对象的内容，被视为右值引用。 有关详细信息，请参阅[右值引用声明符：&&](../cpp/rvalue-reference-declarator-amp-amp.md)。  
  
##  <a name="overflow"></a>  basic_filebuf::overflow  
 当新字符插入到已满缓冲区时调用。  
  
```  
virtual int_type overflow(int_type _Meta = traits_type::eof);
```  
  
### <a name="parameters"></a>参数  
 `_Meta`  
 要插入到缓冲区中的字符或 **traits_type::eof**。  
  
### <a name="return-value"></a>返回值  
 如果该函数不成功，它将返回 **traits_type::eof**。 否则，它将返回 **traits_type::**[not_eof](../standard-library/char-traits-struct.md#not_eof)(_ *Meta*)。  
  
### <a name="remarks"></a>备注  
 如果 _ *Meta***!= traits_type::**[eof](../standard-library/char-traits-struct.md#eof)，则受保护虚拟成员函数将尝试向输出流中插入元素 **ch = traits_type::**[to_char_type](../standard-library/char-traits-struct.md#to_char_type)(\_ *Meta*)。 它可以用多种方法执行此操作：  
  
-   如果写入位置可用，它可将元素存储到写入位置并递增输出缓冲区的下一个指针。  
  
-   它可以通过为输出缓冲区分配新的或额外的存储空间，提供写入位置。  
  
-   它可以根据需要使用文件转换方面 **fac** 调用 **fac.out**，转换输出缓冲区中的任何挂起输出，后跟 **ch**。 因而，生成的类型 *char* 的每个元素 `ch` 将写入文件指针 **fp** 指定的相关流，就像连续调用窗体 `fputc`( **ch**, **fp**) 一样。 如果任何转换或写入失败，该函数不会成功。  
  
##  <a name="pbackfail"></a>  basic_filebuf::pbackfail  
 尝试将元素放回到输入流中，随后使它成为当前元素（由下一个指针指向）。  
  
```  
virtual int_type pbackfail(int_type _Meta = traits_type::eof);
```  
  
### <a name="parameters"></a>参数  
 `_Meta`  
 要插入到缓冲区中的字符或 **traits_type::eof**。  
  
### <a name="return-value"></a>返回值  
 如果该函数不成功，它将返回 **traits_type::eof**。 否则，它将返回 **traits_type::**[not_eof](../standard-library/char-traits-struct.md#not_eof)(_ *Meta*)。  
  
### <a name="remarks"></a>备注  
 受保护虚拟成员函数将元素放回到输入缓冲区中，随后使它成为当前元素（由下一个指针指向）。 如果 _*Meta* **== traits_type::**[eof](../standard-library/char-traits-struct.md#eof)，则要推送回的元素在当前元素之前实际上已是流中的一个元素了。 否则，则由 **ch = traits_type::**[to_char_type](../standard-library/char-traits-struct.md#to_char_type)(\_ *Meta*) 替换该元素。 该函数可以用多种方法放回元素：  
  
-   如果放回的位置可用，且存储在该位置的元素等于 **ch**，则它可以递减输入缓冲区中的下一个指针。  
  
-   如果该函数可以使 `putback` 位置可用，则它可以这样做，将下一个指针指向该位置，并将 **ch** 存储在该位置。  
  
-   如果该函数可以将推送回流元素上输入的流，它可以这样，例如通过调用`ungetc`的类型的元素`char`。  
  
##  <a name="pos_type"></a>  basic_filebuf::pos_type  
 使 basic_filebuf 范围中的此类型等效于 **Tr** 范围中具有相同名称的类型。  
  
```  
typedef typename traits_type::pos_type pos_type;  
```  
  
##  <a name="seekoff"></a>  basic_filebuf::seekoff  
 尝试更改受控制流的当前位置。  
  
```  
virtual pos_type seekoff(off_type _Off,
    ios_base::seekdir _Way,
    ios_base::openmode _Which = ios_base::in | ios_base::out);
```  
  
### <a name="parameters"></a>参数  
 `_Off`  
 要搜寻的相对于 `_Way` 的位置。  
  
 `_Way`  
 偏移操作的起点。 请参阅 [seekdir](../standard-library/ios-base-class.md#seekdir)，查看可能的值。  
  
 `_Which`  
 指定指针位置的模式。 默认允许修改读取和写入位置。  
  
### <a name="return-value"></a>返回值  
 返回新位置或无效的流位置。  
  
### <a name="remarks"></a>备注  
 受保护虚拟成员函数尝试更改受控流的当前位置。 对于类 [basic_filebuf](../standard-library/basic-filebuf-class.md)< `Elem`, `Tr`> 的对象，流位置可以通过类型为 `fpos_t` 的对象表示，它存储偏移量以及分析宽流所需的任何状态信息。 如果偏移量为零，则将指定流的第一个元素。 （类型为 [pos_type](../standard-library/basic-streambuf-class.md#pos_type) 的对象存储至少一个 `fpos_t` 对象。）  
  
 对于打开供读取和写入的文件，输入流和输出流串联放置。 若要在插入和提取之间切换，你必须调用 [pubseekoff](../standard-library/basic-streambuf-class.md#pubseekoff) 或 [pubseekpos](../standard-library/basic-streambuf-class.md#pubseekpos)。 调用 `pubseekoff`（因而调用 `seekoff`）对于[文本流](../c-runtime-library/text-and-binary-streams.md)、[二进制流](../c-runtime-library/text-and-binary-streams.md)和[宽流](../c-runtime-library/byte-and-wide-streams.md)有各种限制。  
  
 如果文件指针 **fp** 为 null 指针，则函数将失败。 否则，它尝试通过调用 `fseek`( **fp**, `_Off`, `_Way`) 更改流位置。 如果该函数成功并且最终位置 **fposn** 可通过调用 `fgetpos`( **fp**, **&fposn**) 来确定，则该函数将成功。 如果函数成功，则返回类型为 **pos_type** 的值，其中包含 **fposn**。 否则，返回一个无效的流位置。  
  
##  <a name="seekpos"></a>  basic_filebuf::seekpos  
 尝试更改受控制流的当前位置。  
  
```  
virtual pos_type seekpos(pos_type _Sp, ios_base::openmode _Which = ios_base::in | ios_base::out);
```  
  
### <a name="parameters"></a>参数  
 `_Sp`  
 要搜寻的位置。  
  
 `_Which`  
 指定指针位置的模式。 默认允许修改读取和写入位置。  
  
### <a name="return-value"></a>返回值  
 如果文件指针 **fp** 为 null 指针，则函数将失败。 否则，它尝试通过调用 `fsetpos`( **fp**, **&fposn**) 更改流位置，其中 **fposn** 是存储在 `pos` 中的对象 `fpos_t`。 如果该函数成功，则该函数将返回 `pos`。 否则，返回一个无效的流位置。 若要确定流位置是否有效，请比较返回值和 `pos_type(off_type(-1))`。  
  
### <a name="remarks"></a>备注  
 受保护虚拟成员函数尝试更改受控流的当前位置。 对于类 [basic_filebuf](../standard-library/basic-filebuf-class.md)\< **Elem**, **Tr**> 的对象，流位置可以通过类型 `fpos_t` 的对象表示，它存储偏移量以及分析宽流所需的任何状态信息。 如果偏移量为零，则将指定流的第一个元素。 （类型为 `pos_type` 的对象存储至少一个 `fpos_t` 对象。）  
  
 对于打开供读取和写入的文件，输入流和输出流串联放置。 若要在插入和提取之间切换，你必须调用 [pubseekoff](../standard-library/basic-streambuf-class.md#pubseekoff) 或 [pubseekpos](../standard-library/basic-streambuf-class.md#pubseekpos)。 调用 `pubseekoff`（因而调用 `seekoff`）对于文本流、二进制流和宽流有各种限制。  
  
 对于宽流，如果自打开流后，或者自上次调用 `streampos` 后发生任何插入，该函数都将调用 [overflow](#overflow)。 它还将根据需要通过使用文件转换方面 **fac** 调用 **fac**`.unshift`，插入任何所需的序列，以还原初始转换状态。 因此，生成的类型 `char` 的每个元素 **byte** 将写入文件指针 **fp** 指定的相关流，就像连续调用窗体 `fputc`( **byte**, **fp**) 一样。 如果调用 **fac.unshift** 或任何写入失败，该函数将不会成功。  
  
##  <a name="setbuf"></a>  basic_filebuf::setbuf  
 执行特定于每个派生流缓冲区的操作。  
  
```  
virtual basic_streambuf<Elem, Tr> *setbuf(
    char_type* _Buffer,  
    streamsize count);
```  
  
### <a name="parameters"></a>参数  
 `_Buffer`  
 指向缓冲区的指针。  
  
 `count`  
 缓冲区的大小。  
  
### <a name="return-value"></a>返回值  
 如果文件指针 `fp` 为 null 指针，则受保护成员函数将返回零。  
  
### <a name="remarks"></a>备注  
 `setbuf` 调用 `setvbuf`( **fp**, ( `char` \*) `_Buffer`, `_IOFBF`, `count` \* `sizeof` ( **Elem**) )，以提供从 _*Buffer* 开始的 `count` 个元素组成的数组作为流的缓冲区。 如果该函数返回一个非零值，则该函数返回 null 指针。 否则，它将返回 **this** 表示成功。  
  
##  <a name="swap"></a>  basic_filebuf::swap  
 将此 `basic_filebuf` 的内容与提供的 `basic_filebuf` 的内容进行交换。  
  
```  
void swap(basic_filebuf& right);
```  
  
### <a name="parameters"></a>参数  
 `right`  
 对另一个 `basic_filebuf` 的 `lvalue` 引用。  
  
##  <a name="sync"></a>  basic_filebuf::sync  
 尝试将受控制流与任何关联的外部流同步。  
  
```  
virtual int sync();
```  
  
### <a name="return-value"></a>返回值  
 如果文件指针 **fp** 为 null 指针，则返回零。 否则，仅当调用 [overflow](#overflow) 和 `fflush`( **fp**) 成功刷新流的任何挂起输出时，才返回零。  
  
##  <a name="traits_type"></a>  basic_filebuf::traits_type  
 将类型名与 **Tr** 模板参数关联。  
  
```  
typedef Tr traits_type;  
```  
  
##  <a name="underflow"></a>  basic_filebuf::underflow  
 从输入流中提取当前元素。  
  
```  
virtual int_type underflow();
```  
  
### <a name="return-value"></a>返回值  
 如果该函数不成功，它将返回 **traits_type::**[eof](../standard-library/char-traits-struct.md#eof)。 否则，它将返回 **ch**，如“备注”部分中所述进行转换。  
  
### <a name="remarks"></a>备注  
 受保护虚拟成员函数尝试从输入流提取当前元素 **ch**，并返回元素作为 **traits_type::**[to_int_type](../standard-library/char-traits-struct.md#to_int_type)( **ch**)。 它可以用多种方法执行此操作：  
  
-   如果读取位置可用，它采用 **ch** 作为存储在读取位置中的元素，并提升输入缓冲区的下一个指针。  
  
-   它可以读取类型的一个或多个元素`char`，像是通过以下形式的连续调用`fgetc`(**fp**)，并将其转换到的元素**ch**类型的**Elem**通过使用文件转换方面预计完成成本调用**fac.in**根据需要。 如果任何读取或转换失败，该函数不会成功。  
  
## <a name="see-also"></a>请参阅  
 [\<fstream>](../standard-library/fstream.md)   
 [C++ 标准库中的线程安全性](../standard-library/thread-safety-in-the-cpp-standard-library.md)   
 [iostream 编程](../standard-library/iostream-programming.md)   
 [iostreams 约定](../standard-library/iostreams-conventions.md)
