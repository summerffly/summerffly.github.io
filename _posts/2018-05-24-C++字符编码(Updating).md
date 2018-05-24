---
layout: post
title: C++字符编码(Updating)
date: 2018-05-24
tags: C++
---

　字符编码的问题，每个程序猿都会遇到，自己刚开始工作的时候总是花费大量时间解决字符编码问题，理顺字符编码的发展历史和机制，理解背后的原理，能让我们少走很多弯路

### 字符编码标准

- **ASCII码**

	计算机最初主要在美国发展，用8个bit组成一个字节(Byte)，8-bit的字节一共可以组合出256(2的8次方)种不同的表示，完全可以满足英语系国家的使用需求，ANSI(American National Standards Institute)于是制定了ASCII编码标准(American Standard Code for Information Interchange，美国信息互换标准代码)，用0~127来表示控制符、标点符号、数字、大小写字母

- **GB2312码**

	ASCII码对于英语系的国家满足使用，但是对于中国这样的拥有上万汉字的国家，1个字节无论如何不可能满足使用，为了表示汉字，同时避免和ASCII编码冲突，规定小于127的字节意义与原来相同，将2个大于127的字节组合表示汉字，前面的一个字节（高字节）从0xA1用到0xF7，后面一个字节（低字节）从0xA1到0xFE，这种汉字方案就是GB2312编码标准

- **GBK码**

	GB2312编码标准可以容纳大约7000多个简体汉字，满足了是日常使用，但是对于一些生僻汉字和繁体字，GB2312编码标准就不够用了，又推出了扩展的GBK编码标准，不再要求低字节要大于127，GBK编码包括了GB2312编码的所有内容，同时又增加了近20000个新的汉字（包括繁体字）和符号

- **Unicode码**

	类似GB2312码这样的编码方式被统称为DBCS（Double Byte Charecter Set，双字节字符集），在DBCS系列标准里，最大的特点是2字节长的汉字字符和1字节长的英文字符并存于同一套编码方案里，解码是必须要注意字串里的每一个字节的值，如果这个值是大于127的，那么就认为一个双字节字符集里的字符出现了，然而新的问题却又出现了，不同地区采用了不同的DBCS编码方案，比如同样显示汉字，台湾就是用了BIG5编码

	为了解决不同的DBCS编码方案冲突的问题，ISO制定了USC编码标准（Universal Character Set，双通用字符集），也就是Unicode码

### 宽字符和窄字符的转换

- **宏A2W() 和 W2A()**

```
// 使用ATL的W2A和A2W宏必须使用USES_CONVERSION
USES_CONVERSION;

// Unicode >>> ASCII
wchar_t* wszText = L"Unicode字符转换为ASCII";
printf("%s\n", W2A(wszText));

// ASCII >>> Unicode
char* szText = "ASCII字符转换成Unicode.";
wprintf(L"%s\n", A2W(szText));
```

- **宏_T("x")**

　_T()是一个适配宏，作用是让程序支持Unicode编码，当#ifdef _UNICODE的时候，_T就是L，没有#ifdef _UNICODE的时候，_T就是ANSI

```
#define _T(x) __T(x)
  ...
#ifdef  _UNICODE
　#define __T(x) L ## x
#else  
　#define __T(x)  x
```

- **宏L"x"**

　不管以什么方式编译，一律以Unicode方式保存

- **WideCharToMultiByte()**

　WideCharToMultiByte()将宽字符转换为窄字符

```
wstring wstr_Src(L"test");
int nBufferSize = WideCharToMultiByte(CP_ACP, 0, wstr_Src, -1, NULL, 0, NULL, NULL);
char *ptrch_Target = new char[nBufferSize];
WideCharToMultiByte(CP_ACP, 0, wstr_Src, -1, ptrch_Target, nBufferSize, NULL, NULL);
string str_Dest(ptrch_Target);
```

- **MultiByteToWideChar()**

　MultiByteToWideChar()将窄字符转换为宽字符

```
string str_Src(L"test");
int nBufferSize = MultiByteToWideChar(CP_ACP, 0, str_Src.c_str(), -1, NULL, 0);
wchar_t *ptrwch_Target = new wchar_t[nBufferSize];
WideCharToMultiByte(CP_ACP, 0, str_Src.c_str(), -1, ptrwch_Target, nBufferSize);
wstring wstr_Dest(ptrwch_Target);
```

