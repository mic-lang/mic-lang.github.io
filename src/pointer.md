# ポインタ
Micではポインタ型は、原則として`mi`ポインタ型を使いましょう。
```c
{
    region p;
    int mi(p)* x = nullptr;
}
```
キャストも利用できます。ただし、グローバル変数のポインタに一時的に関数内のデータを代入する、あるいは、既存のAPIとのwrapperを作るというシチュエーション以外は、あまり望ましくありません。基本的に異なる`mi`修飾子間のキャストはメモリ安全性を壊します。
```
lifetime <region p>
static inline void mic_free(void mi(p)* ptr) {
    mi_free((void*)ptr);
    return;
}
```