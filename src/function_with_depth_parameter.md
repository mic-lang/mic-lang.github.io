# 関数と`depth`パラメータ
Micでは、関数のシグネチャに深さ付きポインタが現れる場合、それらに現れるすべての深さ識別子の順序を浅い方から深い方へ順に`lifetime`構文で明示する必要があります。この際`depth`キーワードを用いて、左から右へとだんだんと深くなっていくように指定します。ここでは、標準Cライブラリのprintf関数を使うために、のちの章で後述するunsafe文を使用してます。

```c
#include <stdio.h>
lifetime <depth p, depth q>
void printer(char p*q* x) {
    for (int i = 0; i != null; i++) {
        unsafe {
            printf("%s", x[i]);
        }
    }
    return;
}

int main () using p {
    char p* hello = "Hello, "; 
    char p* world = "World!\n";
    using q {
        char p* table[3] = {&hello, &world, null};
        printer<p, q>(table);
    }
    return 0;
}
```