```metadata
{
    "title": "pytestのメモ",
    "date": "2022-01-02T19:02:45",
    "categories": "Python",
}```

pytestに関する個人的メモです🙏

## @pytest.mark.parametrizeについて

一つのテストで複数の値を検証することができる

[参考記事: pytestのparametrizeの使い方とその有用性について](https://tech.zeals.co.jp/entry/2019/12/12/131516)



## @pytest.fixtureについて

テスト実行時にデコレータで設定した関数が実行され、その返却値をテストで参照できる、というものです

[参考記事: [Python] 初中級者のためのpytest入門](https://note.crohaco.net/2016/python-pytest/)



## from unittest.mock import patchについて

指定した関数(それ以外も行けるかも？未調査)に値をぶちこめる

こちらの画像が例

use caseなどに任意の値を入れてテストすることが可能なので、pytest.fixtureよりも個人的には大きな発見

![](./Screen-Shot-2022-01-02-at-18.24.46-644x420.png)

[参考記事: pytest ヘビー🐍ユーザーへの第一歩](https://www.m3tech.blog/entry/pytest-summary)

## def test_hoge(client)のclientについて

clientのfixture設定していないのに何故か使えてしまう？？？という問題発生していて、<br>これはpytest-flaskのpip入れているから、らしいです

ちなみに、それ以外にも提供されているfixtureあるので参照してみてください

[参考記事: https://pytest-flask.readthedocs.io/en/latest/features.html#fixtures](https://pytest-flask.readthedocs.io/en/latest/features.html#fixtures)


