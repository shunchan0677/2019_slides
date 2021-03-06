# pandas vor!
### pandasの基礎は奥深いいぞ
#### みんなのPython勉強会#42 (2019/02/13) nikkie

---

### LT: pandas vor!

- 意味：「pandas@color[#79A53A](前進)！」
- pandasを使ったデータ分析を前に進めるため、基礎を確認 → 知ったことを共有します

+++

### About nikkie

- Alias @ftnext: [Twitter](https://twitter.com/ftnext), [はてなブログ](http://nikkie-ftnext.hatenablog.com/)
- 2018/09〜 stapy 4代目LT王子
- 1年前の2018/02が[stapyにおけるはじまりのLT](https://speakerdeck.com/ftnext/begin-pandas)です

+++

### 最近のアウトプット

- Django基礎ハンズオン（eLV勉強会）
  - part1：[2019/01/24](https://elv.connpass.com/event/114810/)、part2：[2019/02/14](https://elv.connpass.com/event/119181/)
- Kaggleタイタニックハンズオン（サポーターズ勉強会）
  - 第1回：[2019/01/30](https://supporterzcolab.com/event/677/)、第2回：2019/03

+++

### LT: pandas vor!

1. indexとcolumn
2. DataFrameで複数列選択し損ねたときのエラー
3. nan、この特別な存在

---

### LT: pandas vor!

1. @color[#79A53A](indexとcolumn)
2. DataFrameで複数列選択し損ねたときのエラー
3. nan、この特別な存在

+++

### indexとcolumn

![indexとcolumnの例：タイタニックの乗客属性データ](stapy_Feb_pandas_basics/assets/df_index_and_column.png)

[PyDataTokyo タイタニックチュートリアル](https://github.com/PyDataTokyo/pydata-tokyo-tutorial-1/blob/master/pydatatokyo_tutorial_dh.ipynb)より

+++

### indexとcolumn

- indexは0, 1, ... の部分（axis=0に対応）
- columnは列名の部分（axis=1に対応）

+++

### `df.drop(['Ticket', 'Cabin'], axis=1)`

@color[#79A53A](カラムの中から)該当する名前を@color[#79A53A](削除)と解釈できる

![カラムの中からTicketとCabinを削除](stapy_Feb_pandas_basics/assets/drop_specified_column.png)

---

### LT: pandas vor!

1. indexとcolumn
2. @color[#79A53A](DataFrameで複数列選択し損ねたときのエラー)
3. nan、この特別な存在

+++

### DataFrameで複数列選択し損ねたときのエラー

- カラム名のリストを渡す：`df[['columnA', 'columnB']]`
- リストで渡し損ねるとKeyError：`df['columnA', 'columnB']`

+++

### なぜKeyError？

- 'columnA', 'columnB'は@color[#79A53A](タプル)として解釈される
- `('columnA', 'columnB')`というカラムを探すが、見つからない

```console
>>> 2, 3
(2, 3)
```

+++

### 試してみる 1/2

DataFrame `prince`

name | alias | (name, alias)
----- | ----- | -----
denzow | denzowill | (denzow, denzowill)
nao_y | NaoY_py | (nao_y, NaoY_py)
Swall0w | Swall0wTech | (Swall0w, Swall0wTech)
nikkie | ftnext | (nikkie, ftnext)

+++

### 試してみる 2/2

`prince['name', 'alias']`

```
0       (denzow, denzowill)
1          (nao_y, NaoY_py)
2    (Swall0w, Swall0wTech)
3          (nikkie, ftnext)
Name: (name, alias), dtype: object
```

[Snippet](https://gist.github.com/ftnext/b4d7946bc99a03a217f1b3a4896cc84c) (Gist)

---

### LT: pandas vor!

1. indexとcolumn
2. DataFrameで複数列選択し損ねたときのエラー
3. @color[#79A53A](nan、この特別な存在)

+++

### nanは特別

- 自分自身と比較してFalse😧
- nan（欠損）の検出は、df.@color[#79A53A](`isnull`)`()`やpd.@color[#79A53A](`isna`)`()`を使いましょう

```python
import numpy as np
np.nan == np.nan
# False
```

---

### Why LT:pandas vor!

なぜこのLTをするに至ったかを共有

+++

### 読み始めました

『[@color[#79A53A](pandasクックブック)](http://www.asakura.co.jp/books/isbn/978-4-254-12242-8/)』(2019 朝倉書店)

![pandasクックブック 書影](http://www.asakura.co.jp/goods_img/115751.jpg)

+++

### どれも@color[#79A53A](pandasクックブック)で知りました

1. indexとcolumn（レシピ1）
2. DataFrameで複数列選択し損ねたときのエラー（レシピ11）
3. nan、この特別な存在 （レシピ17）

+++

### pandasクックブック

- 1-2章でずいぶん知らないことがあった
- 全9章あり、@color[#79A53A](**データ分析の力を鍛えられそう**)でワクワク😆

+++

### まとめ：pandas vor!

- pandasを使ったデータ分析を前進させるために基礎を確認
- 知らないことが多く、@color[#79A53A](pandasの基礎の奥深さ)を実感
- 『pandasクックブック』でpandas基礎力磨きに取り組んでいきます

+++

### ご清聴ありがとうございました。
## pandas vor!
Contact: [Twitter @ftnext](https://twitter.com/ftnext)
