# 関数と`region`パラメータ
Micでは、関数のシグネチャにregion変数が現れる場合、現れるすべてのregion変数を`lifetime`構文を用いて明示する必要があります。この際、region変数の順序は任意に決められます。

```c
#include <stdio.h>
lifetime <region p, region q>
void printer(char mi(p)* mi(q)* x) {
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