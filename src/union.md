# 共用体
構造体と同じく、共用体でも`lifetime`構文が利用できます。

```c
lifetime <region p>
union u_tag {
    int ival;
    float fval;
    char mi(p)* sval;
};
```