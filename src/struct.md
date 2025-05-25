# 構造体
構造体は関数と同じく、`lifetime`構文が利用できます。
構造体のフィールドには`mi`修飾子を使えません。原則として外側の変数に使います。

```c
lifetime <region p>
struct key {
    char mi(p)* name;
    int count;
};

lifetime <region p, region q>
struct tnode {
    char mi(p)* word;
    int count;
    struct tnode mi(q)* left;
    struct tnode mi(q)* right;
};

region r_name;
{
    region p;
    key<r_name> mi(p) key1;
}
```

