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

ここで変数の値が本当に ``None`` かどうかチェックした場合、次のようにします：

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

## 繰り返し（*Iterate*）

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

よくオブジェクトを繰り返し処理する必要がでてきますが、Python であれば簡単に実現できます：

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

関数を定義すると、Python はデフォルト値のインスタンスを生成し、それからオプショナル引数が提供されない場合は常にこのインスタンスを使用します。
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

この書き方は Python ではなにか奇妙な感じがします。私自身はあまり好きな書き方ではありません。今ここで問題として扱ったこの機能は多くの場面でとても便利なのですが、その意味を正しく理解した上で賢く使用する必要があります。

---

## プロパティ vs Getter と Setter

dd理由がなんであれ、ディクショナリ・クラスを独自に実装する必要がある場合、実際には存在していないキーにアクセスしようとして ``KeyError`` を原因とするバグがいろいろと発生することが予想されます。
どのキーが無いのかコードの中をあちこち調べて確認する必要がないように、``KeyError`` が発生する度に呼び出される ``__missing__()`` という特殊なメソッドを実装できます。

```Python
class MyDict(dict):
    def __missing__(self, key):
        message = f'{key} not present in the dictionary!'
        logging.warning(message)
        return message    # もしくは代わりに何かエラーを発行する
```

上の実装はとても単純で、存在しないキーをログ・メッセージとして記録して返すだけですが、他にも貴重な情報をログとして出力しておけば、コードの中の何が間違っているかといった更に詳細な状況を提供することだって可能です。

---

## クラッシュしたアプリケーションのデバッグ

何が起こっているかを確認する前にアプリケーションがクラッシュしてしまった場合、ここで紹介するトリックは非常に有効です。

アプリケーションを ``-i`` オプション付きで起動する（``python3 -i app.py``）と、アプリケーションが終了すると同時にインタラクティブ・シェルモードが開始されます。
このモードで変数や関数を調査することができます。

もしそれでも不十分というのであれば、いっそ「大きなハンマー」〜 ``pdb``（Python デバッガ） 〜 を持ち出してくるという手もあります。
``pdb`` には、それ自体で記事が一つ書けてしまうほど機能が沢山あります。
そのため、ここでは例と一番重要なお手本についての要約を紹介します。
まずはクラッシュを引き起こす小さなスクリプトを見てみることにします：

```Python
# crashing_app.py
SOME_VAR = 42

class SomeError(Exception):
    pass

def func():
    raise SomeError('Something went wrong...')

def main():
    func()

```

ここで ``-i`` オプション付きで実行すると、すぐにデバッグできる状態になります：

```Python
# クラッシュするアプリケーションを実行します
$ python3 -i crashing_app.py
Traceback (most recent call last):
  File "crashing_app.py", line 23, in <module>
    sys.exit(main())
  File "crashing_app.py", line 20, in main
    func()
  File "crashing_app.py", line 17, in func
    raise SomeError('Something went wrong...')
__main__.SomeError: Something went wrong...
>>> # 今、インタラクティブ・シェルモードに居ます
>>> import pdb
>>> pdb.pm()    # Post-Mortem デバッガを起動します
> .../crashing_app.py(17)func()
-> raise SomeError('Something went wrong...')
(Pdb) # ここでデバッガモードに入るので、いろいろ調査したりコマンドを実行できます
(Pdb) p SOME_VAR    # 変数の中身を表示します
42
(Pdb) l    # 現在実行している前後のコードを表示してみます
 12
 13     class SomeError(Exception):
 14         pass
 15 
 16     def func():
 17  ->     raise SomeError('Something went wrong...')
 18 
 19     def main():
 20         func()
 21 
 22     if __name__ == "__main__":
(Pdb)  # このあともデバッグを継続します（ブレークポイントを張ったり、コードをステップ事項したりなど）

```

上で紹介したデバッグ・セッションの例は ``pdb`` を使って何ができるのかを端的に示しています。
まずアプリケーションが終了したあと、対話式のデバッグ・セッションに入ります。
次に ``pdb`` モジュールをインポートしてデバッガを起動します。
この時点で ``pdb`` の全てのコマンドを利用できるようになります。
上の例だと、``p`` コマンドを使って変数の中身を表示し、``l`` コマンドでコードの一覧を表示します。
たいていはブレークポイントをセットするために ``b 行番号`` コマンドを使い、そのブレークポイントに到達するまで ``c`` コマンドを叩いてアプリケーションを実行させ、``s`` コマンドで関数の内部をステップ単位で実行し続け、追加で ``w`` コマンドでスタックトレースを表示することになります。
コマンドの完全な一覧については [pdb のドキュメント](https://docs.python.org/3/library/pdb.html#debugger-commands)をご覧ください。

---

## スタックトレースの調査

例えばリモート・サーバ上で動いている Flask とか Django のような、インタラクティブなデバッグ・モードが使えないアプリケーションのコードがあるとしましょう。
そのような場合は ``traceback`` と ``sys`` パッケージを使えば、コードの中で何が問題になっているのかをさらに詳しく見ることができます：

```Python
import traceback
import sys

def func():
    try:
        raise SomeError('Something went wrong...')
    except:
        traceback.print_exc(file=sys.stderr)
```

これを実行すると、上のコードは最後に例外のメッセージを出力します。
例外が出力されることとは別に、``tracebak`` パッケージを使ってスタックトレースを出力（``traceback.print_stack()``）したり、展開したスタックフレームを整形しさらに検証することも可能です（``traceback.format_list(traceback.extract_stack())``）。


---

## デバッグ中にモジュールを再ロードする

インタラクティブ・シェルモードで何か関数をデバッグしたり検証をしていると、場合によっては頻繁に手を加えることになるかもしれません。
「実行する」、「テストする」、そして「コードを変更する」という一連のサイクルを楽にするために、importlib.reload(_モジュール_) を実行して、コードを変更する度にインタラクティブ・シェルモードのセッションを再起動しなくてもよいようにできます：

```Python
In [11]: import func from module

In [12]: func()
This is result...

# ここで "func()" のコードを変更する
In [13]: func()
This is result...    # これは古いコード

In [14]: from importlib import reload    # "func()" を変更したら "module" をリロードする

In [15]: reload(module)
Out[15]: <module 'module' from '...'>

In [16]: func()
New result..    # これが新しいコード

```

これは「デバッグ」というよりは作業の効率化のヒントです。
いくつかの不要な操作を省略し、作業フローをより速くそして効率よくすることは何時だって素晴らしいことです。
一般的に、モジュールを「時々」再ロードするのは良い考えで、そうこうしている間に何度も変更されたコードをデバッグしようとする羽目にならないようにしてくれます。


「*デバッグは芸術です*」

---

## おわりに

「本当のところプログラミングとは何なのか？」と問われたら、ほとんどの場合、それはまさしく試行錯誤の繰り返しです。
その一方でデバッグ作業はと言うと、（これは個人的な意見ですが）「芸術」であり、上手になるには時間と経験が必要です。
使用するたくさんのライブラリやフレームワークをよく知るほど、それが楽になります。
ここで紹介したヒントや技を使うとデバッグ作業がちょっとだけ効率良くなり、そしてちょっとだけ早く終わりますが、紹介した Python 固有のツールは別にして、デバッグ作業に対する一般的なアプローチを習得してみようと考えるよい機会にもなるかもしれません。
例えば Remy Sharp 著の「[The Art of Debugging](https://remysharp.com/2015/10/14/the-art-of-debugging)」を読んでみるとか。

