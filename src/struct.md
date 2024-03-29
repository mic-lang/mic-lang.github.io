# 構造体
構造体は関数と同じく、`lifetime`構文が利用できます。関数の時と同じように、構造体内に現れるすべての深さ識別子を`depth`キーワードを用いて、左から右へとだんだんと深くなっていくように指定します。

```c
lifetime <depth p, kind a>
struct key {
    char p a* name;
    int count;
};

lifetime <depth p, depth q>
struct tnode {
    char p* word;
    int count;
    struct tnode q* left;
    struct tnode q* right;
};
```