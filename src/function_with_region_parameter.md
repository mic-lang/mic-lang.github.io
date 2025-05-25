# 関数と`region`パラメータ
Micでは、関数のシグネチャにregion変数が現れる場合、現れるすべてのregion変数を`lifetime`構文を用いて明示する必要があります。この際、region変数の順序は任意に決められます。
現実装では、関数の引数自身には、`mi`修飾子は修飾できません。引数の型のみにしか修飾できません。関数の引数のアドレスを取るには、一度関数内の変数に引数の値を移してから、その変数のアドレスをとるようにしましょう。
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