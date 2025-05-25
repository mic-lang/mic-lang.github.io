# 配列型
Micの配列型にも原則として`mi`修飾子で修飾します。
```c
{
  　region p;
    int mi(p) table[10][20];
}
```

これらの違いはポインタ型への変換の際に現れます。
Micでは一般に、深さ`p`のブロック文で定義された配列型の変数は、その配列型の次元によらず、深さ付きポインタ型`int p*`に一律に変換することができます。
MicではC言語と同様に、配列型として定義された識別子（上の例では、`table`）は配列の先頭アドレスを指し示しています。これは、配列型の次元がいくつになっても同様で、その先頭アドレスの型は、配列の要素の型への深さとしてその配列自身が宣言されたブロックの深さを取る深さ付きポインタ型（上の例では、`int p*`）であることが保証されます。
そのため、もし多次元配列を深さ付きポインタ型に変換した場合は、2つ以上の`[]`で添え字アクセスすることが型システム上許されないので、ユーザーは、任意の要素にアクセスするために、1つの`[]`の中で自分で添え字を計算しなければなりません。
そのため、Micでは多次元配列を関数に渡すときは、C言語同様、多次元の配列型で定義された引数によって多次元配列を受け取ることができます。ただし、この場合、その引数は、その配列の次元数分、配列逆参照した結果のみしか読み書きすることができないように制限されています。
```c
#include <stdio.h>

#define mi(i)  __attribute__((__address_space__(i)))
typedef const __region_t region;

lifetime <region p>
int strlen_(char mi(p)* s) {
    int n = 0;
    for (; *s != '\0'; s++)  {
        n++;      
    }
    return n;
}

void arr1(int num[2][3], int row, int col) {
  for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
      printf("%d ", num[i][j]);
    }
      printf("\n");
  }
  return;
}

lifetime <region p>
void arr2(int mi(p)* num, int row, int col) {
  for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
      printf("%d ", num[col*i+j]);
    }
    printf("\n");
  }
  return;
}

int main () {
    region p;
    //region p;
    char mi(p) str[] = "hello world";
    int len = strlen_<p>(str);
    printf("%d \n", len);      
    
    int mi(p) num[2][3] = {
        {1,2,3},
        {4,5,6}
    };
    
    arr1(num, 2, 3);
    arr2<p>(num, 2, 3);
}
```
配列型へのポインタ型を表現するときも、普通の深さ付きポインタ型と同じように深さを明示する必要があります。この場合深さは省略できません。
```c
int (mi(p)* daytab)[13]
int (mi(p)* table)[10][20]
```