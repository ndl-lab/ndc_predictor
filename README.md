# 概要

ndc_predictorは、[fastText](https://github.com/facebookresearch/fastText) を利用した、書誌データからの [NDC(9 版)](https://www.jla.or.jp/committees/bunrui/tabid/789/Default.aspx) 推定学習モデルである。
精度は以下の通りである。

| モデル  | クラス | 精度（R@1） |
| ------- | ------ | ----------- |
| NDC1 桁 | 10     | 0.86        |
| NDC2 桁 | 100    | 0.819       |
| NDC3 桁 | 947    | 0.753       |

# 学習

学習データとしては、国立国会図書館の書誌データ（2017 年 7 月断面）のうち、NDC のある資料（4,256,238 件）のタイトル、出版者、責任表示のテキストを結合して利用している。
形態素解析には、 mecab-ipadic-neologd の辞書と kuromoji を利用している。kuromoji のモードは SEARCH である。

```java
String ndc = "000"
String str = "any text";
List<String> toknes =  new ArrayList<>();
try (JapaneseTokenizer tokenizer_a = new JapaneseTokenizer(null, false, JapaneseTokenizer.Mode.SEARCH)) {
    tokenizer_a.setReader(new StringReader(str));
    CharTermAttribute term = tokenizer_a.addAttribute(CharTermAttribute.class);
    tokenizer_a.reset();
    while (tokenizer_a.incrementToken()) {
        tokens.add(term.toString());
    }
}
out.println("__label__"+ndc+" "+String.join(" ", tokens));
```

# 学習済みfastTextモデル

gzip で圧縮してあり、各 272MB である。

- [NDC 1 桁](https://lab.ndl.go.jp/ndc/model/model_ndc1.bin.gz)
- [NDC 2 桁](https://lab.ndl.go.jp/ndc/model/model_ndc2.bin.gz)
- [NDC 3 桁](https://lab.ndl.go.jp/ndc/model/model_ndc3.bin.gz)

# テストデータ

test 以下に、NDC3 桁モデル用の評価データを置いている。

```bash
./fasttext test model_ndc3.bin test_ndc3.txt
```
