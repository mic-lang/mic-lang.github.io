# 関数と`kind`パラメータ
`kind`パラメータは深さ修飾子を抽象化します。`kind`パラメータを使うことによって、ユーザーは特定の深さ修飾子に依存しない深さ付きポインタ型を関数内で使うことができ、これによって、異なる深さ付きポインタ型に対しての処理を共通化することができます。以下の例では、`kind`パラメータを用いて定義された関数を呼び出す際に、どの深さポインタ型の引数を渡す際にはどんな深さ修飾子を`kind`パラメータに渡すべきかを網羅しています。

```c
lifetime <depth p, kind a>
void swap(void p a* lhs, void p a* rhs) {
    void p a* tmp = lhs;
    lhs = rhs;
    rhs = tmp;
    return;
}

int main () using p {
    static char* x1 = "x1";　
    static char* y1 = "y1";
    //if pointer depth is static and no depth qualifer appear then the kind is also static
    swap<static, static>(x1, y1);
    unsafe {
        printf(x1);
    }

    
    char p* x2 = "x2";
    char p* y2 = "y2";
    swap<p, auto>(x2, y2);
    unsafe {
        printf(x2);
    }
    

    char p dyn* x3 = mi_heap_malloc<p>(p, sizeof char * 3);
    char p dyn* y3 = mi_heap_malloc<p>(p, sizeof char * 3);
    x3[0] = 'x';
    x3[1] = '3';
    x3[2] = '\0';
    y3[0] = 'y';
    y3[1] = '3';
    y3[2] = '\0';
    swap<p, dyn>(x3, y3);
    unsafe {
        printf(x3);        
    }


    return 0;
}

```