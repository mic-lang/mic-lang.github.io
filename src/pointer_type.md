# Micのユニークなポインタ型
この章では、Micのユニークなポインタ型について見ていきます。
Micでは、`region`型と呼ばれる特別な`unsigned int`のtypedefで定義される、region変数と呼ばれる識別子を用いて`mi`ポインタ型を表現します。
それらは、`int mi(p)*`、`int mi(p)* mi(p)*`といったあまり見慣れない形をしているかもしれません。ただ章を進めていけば、すぐにその有用性に気が付くでしょう。