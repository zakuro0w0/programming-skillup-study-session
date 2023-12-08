<h1>chapter-7.SOLID原則</h1>

<!-- TOC -->

- [1. Single Responsibility Principle単一責務の原則](#1-single-responsibility-principle%E5%8D%98%E4%B8%80%E8%B2%AC%E5%8B%99%E3%81%AE%E5%8E%9F%E5%89%87)
    - [1.1. 変数のSRP違反](#11-%E5%A4%89%E6%95%B0%E3%81%AEsrp%E9%81%95%E5%8F%8D)
    - [1.2. ret = bar;反](#12-ret--bar%E5%8F%8D)
    - [1.3. foo;クラスのSRP違反](#13-foo%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%AEsrp%E9%81%95%E5%8F%8D)
    - [1.4. 参考URL](#14-%E5%8F%82%E8%80%83url)
- [2. Open Closed Prinsiple開放・閉鎖の原則](#2-open-closed-prinsiple%E9%96%8B%E6%94%BE%E3%83%BB%E9%96%89%E9%8E%96%E3%81%AE%E5%8E%9F%E5%89%87)
- [3. Interface Segregation Principleインターフェース分離の原則](#3-interface-segregation-principle%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9%E5%88%86%E9%9B%A2%E3%81%AE%E5%8E%9F%E5%89%87)
- [4. Dependency Inversion Principle依存性逆転の原則](#4-dependency-inversion-principle%E4%BE%9D%E5%AD%98%E6%80%A7%E9%80%86%E8%BB%A2%E3%81%AE%E5%8E%9F%E5%89%87)
- [5. 参考URL](#5-%E5%8F%82%E8%80%83url)

<!-- /TOC -->



# 1. Single Responsibility Principle(単一責務の原則)
- モジュール、クラス、関数等の責務を単一とするべし
- 責務が単一 = 変更される理由が単一

## 1.1. 変数のSRP違反
```C++
int main(){
	bool ret = foo();
	if(ret){
		// 1つの変数に異なる出力を再代入している
		// foo()とbar()は戻り値の型こそ同じだが、役割も戻り値の意味も異なる
		// 変数retが表現する役割がコードの実行位置によって異なり、1つに定まらない
		// foo()とbar()、2つの理由によってretの変更が必要になり得る点がSRP違反
		ret = bar();
	}
}
```

## 1.2. ret = bar();反
```C++
int getX(){
	// 如何にもxを返すだけの責務に見える関数で、何か別の処理も実行している
	// 責務の遂行に必要でない処理ならば、同じ関数に含めるべきではない
	// xを返す処理、foo()の呼び出し、2つの理由でgetX()の変更が必要になり得る点がSRP違反
	foo();
	return x;
}
```

## 1.3. foo();クラスのSRP違反
```C++
class SmartPhone{
public:
	
};
```



## 1.4. 参考URL
- [【OOP入門】単一責任を理解しよう(その1) - Qiita](https://qiita.com/yu-croco/items/a5974afbb5c2de7d2346)

# 2. Open Closed Prinsiple(開放・閉鎖の原則)
- [【OOP入門】単一責任を理解しよう(その1) - Qiita](https://qiita.com/yu-croco/items/a5974afbb5c2de7d2346)


# 3. Interface Segregation Principle(インターフェース分離の原則)


# 4. Dependency Inversion Principle(依存性逆転の原則)


# 5. 参考URL
- [SOLID原則 - the SOLID principles - うまとま君の技術めも](https://umatomakun.hatenablog.com/entry/2018/12/06/233610)
- [オブジェクト指向設計原則とは - Qiita](https://qiita.com/UWControl/items/98671f53120ae47ff93a#はじめに)