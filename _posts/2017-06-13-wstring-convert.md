---
layout: post
title: wchar_t 스트링을 char 스트링으로 변환
category: dev
tags: [c++, dev]
---
# Note: <codecvt> 헤더가 c++17에서 deprecated 됨.

```cpp
template< class Elem,
	unsigned long Maxcode = 0x10ffff,
	std::codecvt_mode Mode = (std::codecvt_mode)0 >
class codecvt_utf8 : public std::codecvt<Elem, char, std::mbstate_t>;

template< class Elem,
	unsigned long Maxcode = 0x10ffff,
	std::codecvt_mode Mode = (std::codecvt_mode)0 >
class codecvt_utf16 : public std::codecvt<Elem, char, std::mbstate_t>;

template< class Elem,
	unsigned long Maxcode = 0x10ffff,
	std::codecvt_mode Mode = (std::codecvt_mode)0 >
class codecvt_utf8_utf16 : public std::codecvt<Elem, char, std::mbstate_t>;
```
* codecvt_utf8  
UTF-8로 인코딩된 바이트 스트링 <-> (Elem 타입에 따라) UCS2 혹은 UCS4로 인코딩된 스트링간의 변환  
텍스트&바이너리 모드로 UTF-8파일을 읽거나 쓸 수 있다.

* codecvt_utf16  
UTF-16으로 인코딩된 바이트 스트링 <-> (Elem 타입에 따라) UCS2 혹은 UCS4 인코딩된 스트링간의 변환  
바이너리 모드로 UTF-16파일을 읽거나 쓸 수 있다.  
특히 `codecvt_utf16<wchar_t>`의 경우

  * gcc-6 까지는 구현에 버그가 있어서 이 [예제](http://en.cppreference.com/w/cpp/locale/codecvt_utf16)의 결과가 올바르게 출력되지 않는다. gcc7 은 미확인
  * vc의 경우 wchar_t가 2바이트이므로 앞의 예제 결과에서 3번째 문자는 디코딩에 실패한다.

* codecvt_utf8_utf16  
UTF-8로 인코딩된 바이트 스트링 <-> UTF-16으로 인코딩된 스트링간의 변환  
Elem이 32비트 타입이면 하나의 UTF-16 코드유닛은 각각의 32비트 문자에 저장  
`std::wstring_convert`와 함께 사용

```cpp
template<class _Codecvt,
	class _Elem = wchar_t,
	class _Walloc = allocator<_Elem>,
	class _Balloc = allocator<char> >
class wstring_convert {
	...
	wide_string from_bytes(...);
	byte_string to_bytes(...);
};
```
`wstring_convert`클래스는 wide타입의 문자/문자열을 char타입의 문자/문자열로 서로 변환하는 기능을 제공한다.  
실제 변환은 Codecvt타입 인스턴스가 한다.  
첫번째 템플릿 인자는 codecvt 타입이고 두번째 인자는 wide string의 element의 타입

```cpp
std::string cvt(const wchar_t * s) {
	std::wstring_convert<std::codecvt_utf8_utf16<wchar_t>, wchar_t> conversion;
	return conversion.to_bytes(s);
}
```

wchar_t 타입의 스트링을 입력받아 utf8로 변환한 뒤 std::string 스트링으로 변환한다
