# unsafe文
Micの`unsafe`文では、C言語の構文のプログラムを直接書くことができ、Micの型システムの制限を超える操作ができます。これによって、生のC言語の関数を呼び出す等の操作が可能になり、Micと生のC言語をつなぐインターフェースとして使うことができます。以下は、Micの標準ライブラリ内での`mi_strlen`の定義です。実は、`mi_strlen`は、`unsafe`文の中で、標準Cライブラリの`strlen`関数を呼び出すラッパー関数なのです。

```c
//string.h

lifetime <depth p, kind a>
size_t inline static mi_strlen(const char p a* s) {
    unsafe {
        return strlen(s);
    }
}
```