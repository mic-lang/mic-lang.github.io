# 関数と`region`パラメータ
Micでは、関数のシグネチャに深さ付きポインタが現れる場合、それらに現れるすべての深さ識別子の順序を深さの浅い方から深い方へ順に`lifetime`構文を用いて明示する必要があります。この際`region`キーワードを用いて、左から右へとだんだんと深くなっていくように指定します。ここでは、標準Cライブラリのprintf関数を使うために、のちの章で後述するunsafe文を使用してます。

```c
#include <stdio.h>
lifetime <region p, region1>
void printer(char p*q* x) {
    for (int i = 0; i != nullptr; i++) {
        printf("%s", x[i]);
    }
    return;
}

int main () {
    region p;
    char mi(p)* hello = "Hello, "; 
    char mi(p)* world = "World!\n";
    {
        region q;
        char mi(p)* table[3] = {&hello, &world, nullptr};
        printer<p, q>(table);
    }
    return 0;
}
```