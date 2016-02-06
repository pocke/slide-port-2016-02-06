class: center, middle

<h1>Rubyで<br>ショートコーディング</h1>


<br>


2016/2/6 <a href="http://freestyle-mokumoku.connpass.com/event/25617/">port-mokumoku #13</a>

---

class: center, middle

だれこれ?
=========

---

だれこれ?
----------

.col-xs-6[
## Pocke
Developer for Developers.

- GitHub: [`@pocke`](https://github.com/pocke)
- Twitter [`@p_ck_`](https://twitter.com/p_ck_)
]

.col-xs-6[

.pocke-img[
![pocke](pocke.svg)
]

]


---

class: center, middle

なにしてるの?
==========


---

なにしてるの?
-----------

- <a href="https://bearfruits.net">bearfruits</a>
  - プログラマの為のポートフォリオ生成サービス
- <a href="https://www.sideci.com">SideCI</a>
  - 自動コードレビューサービス

---



LTでよくある…
======


--

- We are hiring!

--

- 弊社ではエンジニアを募集しています!!

---

class: center, middle

でも学生なので…
==========

---

class: center, middle

私はエンジニアの友達を<br>募集しています!
==========

---

class: center, middle

ところで本題
-------


Rubyで<br>ショートコーディング
===============

---

class: center, middle

ショートコーディング<br>とは
========

---


ショートコーディングとは
--------

- コードを短く書く

--

- 短さ is 正義!


---

たとえば…
========

.col-xs-6[
before(164Byte)

```ruby
require 'prime'

while true
  n = gets.to_i
  if n == 0
    break
  end


  primes = Prime.take(n)

  sum = 0
  primes.each do |i|
    sum += i
  end
  puts sum
end
```
]

.col-xs-6[
after(53Byte)

```ruby
#!ruby -nrprime
p Prime.take(eval$_).inject(:+)||exit
```
]


---

class: center, middle

短い!!!!!!
=========

---


例えば、素数の和
-----------


- [素数の和 | Aizu Online Judge](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0053)

.col-xs-6[
input

```ruby
2
9
0
```
]

.col-xs-6[
output

```ruby
5   # => 2 + 3
100 # => 2 + 3 + 5 + 7 + 11 + 13 + 17 + 19 + 23
```
]


---

素直に書くと
-------

最初のコード


```ruby
require 'prime'

while true
  n = gets.to_i
  if n == 0
    break
  end


  primes = Prime.take(n)

  sum = 0
  primes.each do |i|
    sum += i
  end
  puts sum
end
```


---


Step 1: 後置if
-------

Ruby では if を後置して1行で書ける

```ruby
require 'prime'

while true
  n = gets.to_i
  break if n == 0 # ここ!!!!

  primes = Prime.take(n)

  sum = 0
  primes.each do |i|
    sum += i
  end
  puts sum
end
```

164Byte -> 153Byte (マイナス11Byte)


---

Step 2: inject
------

Ruby では合計値を出すために`inject`メソッドが使える

```ruby
require 'prime'

while true
  n = gets.to_i
  break if n == 0

  primes = Prime.take(n)

  sum = primes.inject(:+) # ここ!!!!
  p sum
end
```

153Byte -> 126Byte (マイナス27Byte)

---

Step 3: コマンドラインオプション
-----------

Ruby では`-n`という[便利オプション](http://docs.ruby-lang.org/ja/2.3.0/doc/spec=2frubycmd.html)がある。

1行ずつ標準入力から読み込むループをつくる。

```ruby
while gets
  # ここで $_ という変数で読み込んだ値が使える
end
```

---

Step 3: コマンドラインオプション
-----------

`while` が消えて、`#!ruby -n`になった

```ruby
#!ruby -n
require 'prime'

n = $_.to_i
break if n == 0

primes = Prime.take(n)

sum = primes.inject(:+)
p sum
```

126Byte -> 110Byte (マイナス16Byte)

---

Step 3: コマンドラインオプション
----------

`require`も`-r`オプションを使うと短くなる

```ruby
#!ruby -nrprime

n = $_.to_i
break if n == 0

primes = Prime.take(n)

sum = primes.inject(:+)
p sum
```

110Byte -> 99Byte (マイナス11Byte)

---

Step 4: 空行を消して整理
---------

空行を消して、`primes`, `sum`という中間変数も消してみる

```ruby
#!ruby -nrprime
n=$_.to_i
break if n==0
p Prime.take(n).inject(:+)
```

99Byte -> 66Byte (マイナス33Byte)

---


Step 5: break を消す
------

空配列の場合、`inject`は`nil`を返すのを利用する

```ruby
#!ruby -nrprime
p Prime.take($_.to_i).inject(:+)||exit
```

66Byte -> 54Byte (マイナス12Byte)

---

Step 6: .to_i は長い
------

`.to_i`(5文字)よりも`eval`(4文字)の方が1Byte短い

```ruby
#!ruby -nrprime
p Prime.take(eval$_).inject(:+)||eixt
```

54Byte -> 53Byte (マイナス1Byte)


---

class: center, middle


まとめ
=======


---

まとめ
------

- コマンドラインオプションは便利

--

- 標準に用意されている関数を使うと短くなる

--

- 気合
