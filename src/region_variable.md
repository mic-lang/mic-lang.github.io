# region変数

`region`は特別な`unsigned int`のtypedefです。`region`型の変数、すなわちregion変数は何も初期化子を指定しなければ、かならずその変数が属するブロックの深さで初期化されます。
この型の変数の値は、グローバル変数なら0で、関数の1番外側のブロックだと1,その後は、ブロックが1つネストするにつれて1ごと大きくなります。
残念ながら、現実装ではグローバル変数は明示的に0で初期化しないといけません。

```c
#include <stdio.h>

const region _static = 0;
int main()
{
    region p;
    printf("%d\n", p);
    {
        region q;
        printf("%d\n", q);
    }
    return 0;
}

```
実行結果：
```
1
2
```