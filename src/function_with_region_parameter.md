# 関数と`lifetime`構文
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
`lifetime` templateがどこにもインスタンス化されていない場合は、明示的にインスタンス化しなければなりません。このとき関数のシグネチャに含まれる任意の`mi`修飾子には`0`か`_static`を渡すのが望ましいでしょう。

```c
#include "mic.h"

lifetime <region p>
void test_heap(void mi(p)* p_out) {
  region q;
  mi_heap_t mi(q)* heap = mic_heap_new<q>();
  void mi(q)* p1 = mic_heap_malloc<q>(heap,32);
  void mi(q)* p2 = mic_heap_malloc<q>(heap,48);
  mic_free<p>(p_out);
  mic_free<q>(p1); mic_free<q>(p2);
}

lifetime void test_heap(void mi(_static)* p_out);
```