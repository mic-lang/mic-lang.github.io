# 配列型
Micの配列型にも原則として`mi`修飾子で修飾します。
```c
{
  　region p;
    int mi(p) table[10][20];
}
```

Micでは一般に、`mi`修飾子`mi(p)`で修飾された配列型の変数は、その配列型の次元によらず、`mi`ポインタ型`int mi(p)*`に一律に変換することが望ましいです。
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