# mi修飾子

Micの大きな、そして、固有な特徴は`mi`修飾子の導入です。
`mi`修飾子はcv修飾子と同じ位置に現れることができる修飾子で型や変数を修飾します。
`mi`修飾子はregion変数を一つ引数に取ります。
変数を`mi`修飾子で修飾するときは、その変数が属するブロックで定義されたregion変数を`mi`修飾子に渡します。
`mi`修飾子で修飾された変数のアドレスをとると、その`mi`修飾子で修飾された`mi`ポインタ型になります。
```c
{
    region p;
    int x mi(p) = 0;
    {
        region q;
        int mi(p)* ptr1 = &x; // ok!
        int mi(q)* ptr2 = &x; // error: the type of &x is int p*, which is incompatible with int q*, the type of p2 
    }
}
```

