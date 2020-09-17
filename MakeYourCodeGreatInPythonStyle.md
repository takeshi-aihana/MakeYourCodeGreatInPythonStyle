# あなたのコードを素晴らしい Python スタイルにする

* (原文: [Make Your Code Great, Python Style＠Live Code Stream](https://livecodestream.dev/post/2020-06-08-make-your-code-great-python-style)）

---

## はじめに

『もっと良い Python コードを書くためのヒントを学ぶ』。

Python は素晴らしいプログラミング言語の一つであり、自分たちのコードをもっと読みやすく、もっと簡潔に、そしてもっとイケてるものにするための素晴らしいツールをいろいろと提供してくれます。
今日は、さらに「Python らしい（*Pythonic*）」コードを書く方法を紹介したいと思います：
この記事では、あなたのコードの品質を改善してくれる素晴らしいヒントを幾つか網羅しています。
では始めましょう。

---

## アンパックを使おう

Python では、変数のリストやタプルを演算子の代入側におくことができます。
これによりコードが簡単になり、さらに読みやすくなります。

ではタプルのアンパックの例から始めましょう：

```python
In [1]: a, b, c = (1, 2, 3)

In [2]: a

Out[2]: 1

In [3]: b

Out[3]: 2

In [4]: c

Out[4]: 3

```

簡単に言うと、代入演算子の左側に１個以上の変数をおくと、右側にある値が一つずつ対応する変数に代入されます。
ここで注意すべき点は、代入演算子の左側と右側におくアイテムの数を同じにするということです。
さもなくば、次のような ``ValueError`` と言うエラーが発生します：

```Python
In [5]: a, b = (1, 2, 3)
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-5-faee755feda6> in <module>
----> 1 a, b = (1, 2, 3)

ValueError: too many values to unpack (expected 2)

In [6]: a, b, c = (1, 2)
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-6-24077ba23852> in <module>
----> 1 a, b, c = (1, 2)

ValueError: not enough values to unpack (expected 3, got 2)

```

ところが、これが Python の驚くべき機能になるのですが、このエラーを回避する方法がいくつかあります。
例えば次のように書くことができます：

```Python
In [7]: a, *b, c = 1, 2, 3, 4, 5, 6

In [8]: a

Out[8]: 1

In [9]: b

Out[9]: [2, 3, 4, 5]

In [10]: c

Out[10]: 6

```

何が起こったのか分かりますか？
このコンテキストで ``*`` 演算子を使うと、アンパックの機能に拡大され、一個の変数の中に複数の値を格納（パック）することができるようになります。
まさに式によって使われなかった一回限りです。
これは驚きです。
``SyntaxError`` にならないように、代入で使用できる ``*`` 演算子は１個だけであることは忘れないようにして下さい：

```Python
In [11]: a, *b, c, *d = 1, 2, 3, 4, 5, 6
  File "<ipython-input-11-895fa2c942c1>", line 4
SyntaxError: two starred expressions in assignment

```

さらにリストをアンパックすることもできます：

```Python
In [12]: a, b, c = ['a', 'b', 'c']

In [13]: a
Out[13]: 'a'

```

文字列も同様です：

```Python
In [14]: a, b, c = 'def'

In [15]: a
Out[15]: 'd'

```

実際のところ、すべてのイテラブルでアンパックの機能が使えます。
但し、次のヒントに進む前に、もっとすごいことをお教えしましょう：

```Python
In [16]: a = 1

In [17]: b = 2

In [18]: a, b = b, a

In [19]: a
Out[19]: 2

In [20]: b
Out[20]: 1

```

こんなエレガントな変数の交換を今まで見たことあります？

---

## None をチェックする

Python における ``None`` キーワードは ``NULL`` 値または全く値が無いことを定義する際に使用します。
他のプログラミング言語とは異なり、Python でいう ``None`` は ``NoneType`` と言うデータ型で、``None`` だけが ``None`` になります。
それではサンプルコードの中で ``None`` がどのように動いているのか見てみることにしましょう：

```Python
In [1]: x = None

n [2]: type(x)
Out[2]: NoneType

In [3]: x == 0
Out[3]: False

In [4]: x == False
Out[4]: False

In [5]: 

```

ここで変数の値が本当に ``None`` かどうかをチェックしたいのであれば、次のようにします：

```Python
In [5]: x == None
Out[5]: True

```

この方法は間違いではないですが、もっと「Python らしい」方法があります：

```Python
In [6]: x is None
Out[6]: True

In [7]: x is not None
Out[7]: False

```

前者も後者も同じことをしていますが、後者の方がより人間にわかりやすいコードに見えます。

---

## 繰り返しの処理（*Iterate*）

他に Python が優れていることを例示するすばらしいサンプルとして Python の繰り返し（*iteration*）がありますが、これははとても洗練されたテクニックである、もしくはひどく「Python らしくない（*UnPythonic*）」（この造語が使われていたとしても）かのどちらかです。

もしあなたが Javascript やＣ言語の経験者であるならば、どのようにして Python のループ処理を実現しますか？
私の場合はこんな具合です：

```Python
In [8]: x = [1, 2, 3, 5, 8, 11]

In [9]: for i in range(len(x)):
   ...:     print(x[i])
   ...: 
1
2
3
5
8
11

```

さらに別のオプションも学習しました：

```Python
In [10]: x = [1, 2, 3, 5, 8, 11]

In [11]: for i in x:
    ...:     print(i)
    ...: 
1
2
3
5
8
11

```

でも本当にあなたが知りたいのは逆順で繰り返しではないですか。こんな感じで：

```Python
In [12]: x = [1, 2, 3, 5, 8, 11]

In [13]: for item in x[::-1]:
    ...:     print(item)
    ...: 
11
8
5
3
2
1

```

これはとてもイイ感じですが、見た感じがまだ変です。
人にやさしいようには見えません。
おそらく別のやり方があります：

```Python
In [14]: x = [1, 2, 3, 5, 8, 11]

In [15]: for item in reversed(x):
    ...:     print(item)
    ...: 
11
8
5
3
2
1

```

やっと美しくなりました！
が、インデックスとアイテムの値の両方が必要な場合はどうでしょう？
最初の例が全てで、今は全てなくなったように思えます。
心配は無用です。
実は Python らしい方法があるのです：

```Python
In [16]: x = [1, 2, 3, 5, 8, 11]

In [17]: for i, item in enumerate(x):
    ...:     print(i, item)
    ...: 
0 1
1 2
2 3
3 5
4 8
5 11

```

しかし繰り返し処理したい配列が１つ以上あったとしたらどうしますか？
パック/アンパックの「仲間」を使います：

```Python
In [18]: names = ['Juan', 'Gera', 'Martin']

In [19]: ages = [33, 30 , 36]

In [20]: for person in zip(names, ages):
    ...:     print(person)
    ...: 
('Juan', 33)
('Gera', 30)
('Martin', 36)

```

お次は各データに個別にアクセスしたいケース：

```Python
In [21]: names = ['Juan', 'Gera', 'Martin']

In [22]: ages = [33, 30 , 36]

In [23]: for person in zip(names, ages):
    ...:     name, age = person
    ...:     print(name, age)
    ...: 
Juan 33
Gera 30
Martin 36

```

あるいは、さらに一歩進めて：

```Python
In [24]: names = ['Juan', 'Gera', 'Martin']

In [25]: ages = [33, 30 , 36]

In [26]: for name, age in zip(names, ages):
    ...:     print(name, age)
    ...: 
Juan 33
Gera 30
Martin 36

```

たまにオブジェクトを繰り返し処理しなければならないことがありますが、Python であれば簡単に実現できます：

```Python
In [27]: obj = {'name':'Juan', 'age':33}

In [28]: for k in obj:
    ...:     print(k, obj[k])
    ...: 
name Juan
age 33

```

また、オブジェクトの ``.items()`` メソッドを使うとキーと値のペアを一つずつ取得できます：

```Python
In [29]: obj = {'name':'Juan', 'age':33}

In [30]: for k, v in obj.items():
    ...:     print(k, v)
    ...: 
name Juan
age 33

```

さらにオブジェクトは ``.keys()`` と ``.values()`` といったメソッドも提供しており、場合によっては非常に役に立つこともあります。
Python は繰り返しの処理を実現する方法をいろいろ提供しています。
必要なのが単なるループの場合であったとしても、``range()`` メソッドを使っても良いですし、他の方法が開発者にとっては読みやすいのであれば、可能ならばそれを使うべきです。

---

## 変更が可能なオプションは使わないようにする

他のプログラミング言語と同様に、Python は引数のオプション化をサポートしており、それらは非常に手軽なため想定外の動きが発生する可能性もあります。次の例を見て下さい：

```Python
In [31]: def add_value(value, seq=[]):
    ...:     seq.append(value)
    ...:     return seq
    ...: 

```

``add_value()`` という関数があります。この関数を呼び出す度に任意の値をリストに追加し、最後にそのリストを返します。ここまでは問題ないでしょう。
返り値のリストはオプショナル引数です。
では次の関数を呼び出して動きを確認してみましょう：

```Python
In [32]: add_value(5)
Out[32]: [5]

```

すばらしい。
このオプショナル引数はちゃんと動作しています。結果として、一個の値を持つリストを受け取りました。
では、さらに値を追加していきましょう：

```Python
In [33]: add_value(8)
Out[33]: [5, 8]

In [34]: add_value(13)
Out[34]: [5, 8, 13]

```

えっと、ちょっと待って...
これは正しくないように見えますが、実際の結果ですし、それを説明すると理にかなっていますし、そうでも無いかもしれません...。
確認してみましょう。

Python で関数を定義すると、デフォルト値のインスタンスを生成し、それからオプショナル引数が提供されない場合は常にこのインスタンスを使用します。
これが我々の考えているユースケースでは大きな問題です。なぜなら、この特定のケースで必要ではないのに（【訳注】オプショナル引数を指定していないのに同じリストが使われ続けて）どんどんリストが大きくなってしまいます。
それでは、どのように修正していけばよいでしょう？
次のようにすることができます：

```Python
In [40]: def add_value(value, seq=None):
    ...:     if seq is None:
    ...:         seq = []
    ...:     seq.append(value)
    ...:     return seq
    ...: 
    
```

この書き方は Python ではなにか奇妙に見えますし、私自身はあまり好きな書き方ではありません。今ここで問題として扱ったこの機能は、多くの場面でとても便利なのですが、その意味を正しく理解した上で賢く使用する必要があります。

---

## プロパティ vs Getter と Setter

注意：このテーマは Java の開発者らとの間で論争に発展する可能性があります :P
ここで「冗談」以外の話しは、もしあなたが Java の開発者である、またはＣ++を習得している開発者で、次に示すことをしようとしているのであれば、とてもワクワクすることでしょう：

```Python
In [45]: class Person:
    ...:     def get_name(self):
    ...:         return self.__name
    ...:
    ...:     def set_name(self, name):
    ...:         self.__name = name
    ...: 

In [46]: person = Person()

In [47]: person.set_name('Juan')

In [48]: person.get_name()
OUt[48]: Juan

```

さて、何も問題が無くても、これは Python のやり方ではありません。
Python のやり方を説明する前に、ボクシングのグローブを用意しましょう :)

```Python
In [50]: class Person:
    ...:     @property
    ...:     def name(self):
    ...:         return self.__name
    ...: 
    ...:     @name.setter
    ...:     def name(self, value):
    ...:         self.__name = value
    ...: 

In [51]: person = Person()

In [52]: person.name = 'Juan'

In [53]: person.name
Out[53]: 'Juan'

```

よし、これで殴り合いする準備ができました :D

---

## Protected と Private のプロパティ（でも本当はそうではないです...）

任意のオブジェクト内からしかアクセスすることができない "Protected" や "Private" といったインスタンス変数は Python には存在しませんが、これらのプロパティを指定するために全ての Python 開発者が使用する「慣例」があります。

```Python
In [54]: class Test:
    ...:     def __init__(self, *args):
    ...:         self.x, self._y, self.__z = args
    ...: 

In [55]: test = Test(1, 2, 3)

```

ここで、もしクラスのブロック外からプロパティの ``x`` にアクセスしようとすると、``x`` に格納された値を取得します。
これは間違いではなく、そして良い慣習です。

```Python
In [56]: test.x
Out[56]: 1

```

``_y`` に対しても同じことができます：

```Python
In [57]: test._y
Out[57]: 2

```

結果を得ることはできますが、これは悪い慣習です。なぜなら ``_`` で始まっているプロパティはクラスの外側からアクセスすることはできないとクラスの開発者が意図したものだからです。

では ``__z`` の場合はどうでしょう：

```Python
In [58]: test.__z
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-58-ae374415690a> in <module>
----> 1 test.__z

AttributeError: 'Test' object has no attribute '__z'

```

この場合はエラーになりますが、依然としてプロパティにアクセスすることは可能なので、コードにちょっとした「魔法」をかける必要ありそうです。

```Python
In [59]: test._Test__z
Out[59]: 3

```

プロパティの前に ``_クラス名`` を付けると値を参照できますが、こうするのはひどい間違いです。

これに関して [Python のドキュメント](https://docs.python.org/ja/3/tutorial/classes.html#private-variables)によると：

> a name prefixed with an underscore (e.g. _spam) should be treated
> as a non-public part of the API (whether it is a function, a method
> or a data member). It should be considered an implementation detail
> and subject to change without notice.
> 
> Any identifier of the form __spam (at least two leading
> underscores, at most one trailing underscore) is textually replaced
> with _classname__spam, where classname is the current class
> name with leading underscore(s) stripped
>
> （日本語版）
>
> アンダースコアで始まる名前 (例えば _spam) は、(関数であれメソッドであれ
> データメンバであれ) 非 public なAPIとして扱います。これらは、予告なく
> 変更されるかもしれない実装の詳細として扱われるべきです。
>
> __spam (先頭に二個以上の下線文字、末尾に一個以下の下線文字) という形式
> の識別子は、 _classname__spam へとテキスト置換されるようになりました。
> ここで classname は、現在のクラス名から先頭の下線文字をはぎとった名前に
> なります。

---

## リソースを扱う際はコンテキスト・マネージャを使おう

ファイルやデータベースのコネクションといったいろいろなリソースを処理する場合、それらのリソースを正しくクローズする、あるいは解放するといったコードを扱う必要がでてきますが、次のようなコードをよく見かけます：

```Python
In [61]: myfile = open('filename', 'w')

# ここで my_file に書き込んだり、それを使って何か処理する

In [62]: myfile.close()


```

二つのコードの間で何か想定外のことが発生しなければ、このコードは問題ありません。
しかし、もし何かエラーが発生した場合、ファイルが確実にクローズされていることをどのようにしたら確認できるでしょうか？
そこで「コンテキスト・マネージャ」の登場です：

```Python
In [63]: with open('filename', 'w') as my_file:
    ...:    # my_file を使って何か処理する
```

これが答えです。

---

## おわりに

Python はとても簡単に上品なコードを書くことができるプログラミング言語です。
習得が容易であることが学生やプログラミングを学習しようとする人たちの間で非常に人気がありますが、きちんと正しい Python のコードを書くことは非常に重要なポイントです。
この記事を読んで「Pythonらしい」コードを書く方法についてアイディアが幾つか得ることができ、もっとたくさん勉強したくなるものと願っています。

楽しんでいただけましたか！
