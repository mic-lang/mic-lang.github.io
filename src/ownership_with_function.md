# 関数と所有権システム
関数に渡された引数が持つ所有権が、その関数を呼び出すと失われる場合はその引数の深さ付きポインタ型を特別なCV修飾子である`drop`修飾子で修飾する必要があります。

```c
lifetime <depth p, kind a>
void p a* move(void p a* drop x) {
    return x;
}
```

また、パフォーマンスのために`free`関数を渡した後に`null`を代入することを避けたい場合も、`drop`修飾子を使って引数の所有権が失われることを示すことができます。
```c
lifetime <depth p>
void delete(void p dyn* drop x) {
    free(x);
}
```