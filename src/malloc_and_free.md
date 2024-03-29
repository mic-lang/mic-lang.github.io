# 動的メモリ確保
さて、Micではヒープ領域のメモリを管理するために、mimallocのライブラリ関数`mi_heap_malloc`,`mi_free`関数を使います。
これらは、Micの標準ライブラリ`stdlib.h`内でそれぞれ次のように定義されています。
```c
lifetime <depth p>
void p dyn* mi_heap_malloc(depth p, size_t size);

lifetime <depth p>
void mi_free(void p dyn* ptr);
```
ここで、`lifetime`, `depth`といった見慣れない予約語が登場しましたが、これらはC++でいう`template`構文との類推で作られた予約語です。これらは、関数の外側で定義された深さ識別子が関数のシグネチャ内で現れる際に使用されます。この構文の詳細については、次の関数の章で学びます。ここでは、この二つの関数の使い方についてみていきましょう。

## `mi_heap_malloc`関数
Micでは、どんなアドレスもある特定の深さに紐づけられていて、それによってアドレス自体の寿命が決まっています。
それはヒープ領域から確保されたメモリアドレスについても同様であり、Micでは、複数のヒープ領域が存在し、一つ一つのヒープ領域はそれぞれある一つの深さと紐づけられていて、同じ深さのメモリアドレスはある一つのヒープ領域の下で一元的に管理されます。ヒープ領域とその下で管理されているメモリアドレスの寿命は、ヒープ領域に紐づけられている深さによって決まります。このヒープ領域は、対応している深さのブロック文の処理が終了するのに伴って、領域ごと一気に解放されます。
つまり、すべての`mi_heap_malloc`関数でメモリ確保されたメモリアドレスに対して後述する`mi_free`関数を呼び出さなくても、`mi_heap_malloc`関数に渡した深さのブロック文が終了したタイミングで自動的にすべてのその深さに紐付けられたメモリは領域ごと解放されます。
また、Micの`mi_heap_malloc`関数には、関数がどの深さのヒープ領域からメモリを確保するか選択できるようするために。深さ識別子を渡す必要があります。
ただし、深さ識別子は特別な識別子であり変数ではないので、関数の仮引数や実引数部分にしか現れません。
```c
using p {
    int p dyn* x = mi_heap_malloc<p>(p,sizeof int);
    int p dyn* y = mi_heap_malloc<p>(p,sizeof int);
    int p dyn* z = mi_heap_malloc<p>(p,sizeof int);
    mi_heap_malloc<p>(p,sizeof int);
}　//here, all allocation above including even the last one are automatically freed.
```
## `mi_free`関数
あるアドレスを`mi_free`関数に渡すと、そのアドレスに所有権を持っていた変数はすべて、そのアドレスに対する所有権を失います。一番最後に所有権を持っていた変数が寿命を終えても、その一つ前に持っていた変数にはそのアドレスの所有権は二度と戻りません。
ある分岐や複合文の中で、ある変数を`mi_free`関数に渡した場合、その分岐や複合文が終わる前までにその変数に`null`を代入しなければいけません。

```c
using p {
    int p dyn* x = mi_heap_malloc<p>(p,sizeof int);
    mi_free(x);
    x = null;
}
```