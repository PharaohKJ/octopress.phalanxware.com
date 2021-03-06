---
layout: post
title: "IEEE 754 の double しかない世界と反省"
date: 2016-09-24 09:44:54 +0900
comments: true
published: true
categories: JavaScript 
---

普通のプログラマであれば、整数で値を決めたい場合、浮動小数点演算を使いることはない。`x`の`30%`といわれれば `x * 0.3` とせず(精度指定文字リテラルは省く & 小数以下は切り捨てるとして) `x * 30 / 100` としたりして、とにかくなるべく使わない。

しかし 今回 AngularJS の入力フォームからパーセントな値を受け取った(バリデータも使えるし、htmlのstep属性とかも使えて便利だしね)ら、 JavaScript は IEEE 754 の double しかない世界である。

[IEEE 754 - Wikipedia](https://ja.wikipedia.org/wiki/IEEE_754)

Chrome のデバッグコンソールを出して以下を実行すればすぐ試せる。

```JavaScript
> 1/0.7
< 1.4285714285714286
```

こ、、、こんな精度いるぅ? 言われてみると俺の愛する C++ は精度指定なし小数点付き数字は精度 double だった。そして Java もそうである。最近は Java をやっててたまたまIDE開いていたのでそれで試す。

```Java
    System.out.println(1.0/0.7);
    // -> 1.4285714285714286    
```

つらい。どれぐらいつらいかというとこのぐらいのつらさであった

```JavaScript
> 16.40*100
< 1639.9999999999998

> Math.floor(16.40*100)
< 1639

```

`16.40` を 100倍して小数以下切り捨てたら `1639`! IEEE 754 だとそうだっつーのか! マジかよ! 信じない! Java 先生にお伺いをたててやる！

```Java
    System.out.println(16.40D*100);
    // -> 1639.9999999999998
    
    System.out.println(16.40D*100D);
    // -> 1639.9999999999998
    
    System.out.println(16.40D*100F);
    // -> 1639.9999999999998
    
    System.out.println(16.40F*100);
    // -> 1640.0
    
    System.out.println(16.40F*100D);
    // -> 1639.9999618530273
    
    System.out.println(16.40F*100F);
    // -> 1640.0
    
    // DやFを省略すると D(double)扱いになるので指定なしは省略
```

やはり IEEE 754 の double の世界ではこれが正しいらしい。ちなみに以下のような有様である。

```Java
    System.out.println(16.40 *      1);  // ->      16.4 
    System.out.println(16.40 *     10);  // ->     164.0 
    System.out.println(16.40 *    100);  // ->    1639.9999999999998 
    System.out.println(16.40 *   1000);  // ->   16400.0 
    System.out.println(16.40 *  10000);  // ->  164000.0 
    System.out.println(16.40 * 100000);  // -> 1639999.9999999998 
```

今回得た教訓は

- JavaScript はどうがんばっても Number とは IEEE 754 double であり、それしかない世界である
- Excel から計算の移植は楽勝だと思ったが、小数絡んだ計算をするととたんに大変になる
  - 単純な計算といっても小数の精度は調査すること
  - 大変じゃなくなるようなライブラリをちゃんと知っておくこと
- LibreOffice と Microsoft Excel では日付計算周りが値が違うことがある
  - OneDrive でちゃんと確認せよ
  - これも日付計算ライブラリをちゃんと知っておくこと

Qiitaに書くべきだったかな?
