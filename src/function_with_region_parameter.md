# 関数と`region`パラメータ
Micでは、関数のシグネチャにregion変数が現れる場合、それらに現れるすべてのregion変数を`lifetime`構文を用いて明示する必要があります。この際、region変数の順序は任意に決められます。ここでは、標準Cライブラリのprintf関数を使うために、のちの章で後述するunsafe文を使用してます。

```c
#include <stdio.h>
lifetime <region p, region q>
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