# 動的メモリ確保
Micではヒープ領域のメモリを管理するために、mimallocのライブラリ関数のwrapper関数である`mic_heap_new`, `mic_heap_malloc`,`mic_free`,`mic_heap_destroy` 関数を使います。
これらは、Micの標準ライブラリ`mic.h`内でそれぞれ次のように定義されています。
```c
lifetime <region p>
mi_heap_t mi(p)* mic_heap_new(void);

lifetime <region p>
void mic_heap_destroy(mi_heap_t mi(p)* heap);

lifetime <region p>
void mi(p)* mic_heap_malloc(mi_heap_t mi(p)* heap, size_t size);

lifetime <region p>
void mic_free(void mi(p)* ptr);
```

## `mic_heap_new`関数
Micでは、`mic_heap_new`関数を使ってヒープ領域を作成することができます。
このヒープ領域に中に確保したメモリは`mic_heap_destory`関数を使って一度に一気に開放することができます。
```c
{
    region p;
    mi_heap_t mi(p)* heap = mic_heap_new<p>();
}　
```
## `mic_heap_malloc`関数
`mic_heap_new`関数で確保したヒープ領域の中に、`mic_heap_malloc`を使ってメモリを確保します。
```c
{
    region p;
    mi_heap_t mi(p)* heap = mic_heap_new<p>();
    int mi(p)* x = mi_heap_malloc<p>(heap,sizeof int);
}　//here, all allocation above including even the last one are automatically freed.
```
## `mic_free`関数
`mic_heap_malloc`関数で確保されたメモリを早期解放するには、`mic_free`関数を使います。
```c
{
    ...

    int mi(p)* x = mic_heap_malloc<p>(heap, sizeof int);
    mic_free(x);
}

```
## `mic_heap_destory`関数
ブロックを抜けるときは、`mic_heap_destory`関数を使って、そのブロックで確保されたヒープ領域を一気に開放します。
この処理は必ず各ブロックの終わりで一度だけ行います。そうしないとメモリリークします。
```c
{
    region p;
    mi_heap_t mi(p)* heap = mic_heap_new<p>();
    int mi(p)* x = mi_heap_malloc<p>(heap,sizeof int);
    int mi(p)* y = mi_heap_malloc<p>(heap,sizeof int);
    int mi(p)* z = mi_heap_malloc<p>(heap,sizeof int);
    mi_heap_malloc<p>(heap,sizeof int);
    mic_heap_destroy(heap);
    //here, all allocation above including even the last one are automatically freed.
}　
```