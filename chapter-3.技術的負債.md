<h1>chapter-3.技術的負債</h1>

<!-- TOC -->

- [1. 技術的負債とは何か](#1-%E6%8A%80%E8%A1%93%E7%9A%84%E8%B2%A0%E5%82%B5%E3%81%A8%E3%81%AF%E4%BD%95%E3%81%8B)
- [2. 技術的負債の特性・効果](#2-%E6%8A%80%E8%A1%93%E7%9A%84%E8%B2%A0%E5%82%B5%E3%81%AE%E7%89%B9%E6%80%A7%E3%83%BB%E5%8A%B9%E6%9E%9C)
    - [2.1. 負債コードは"わからない"](#21-%E8%B2%A0%E5%82%B5%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AF%E3%82%8F%E3%81%8B%E3%82%89%E3%81%AA%E3%81%84)
    - [2.2. 次の変更に掛かる工数を無駄に増やす](#22-%E6%AC%A1%E3%81%AE%E5%A4%89%E6%9B%B4%E3%81%AB%E6%8E%9B%E3%81%8B%E3%82%8B%E5%B7%A5%E6%95%B0%E3%82%92%E7%84%A1%E9%A7%84%E3%81%AB%E5%A2%97%E3%82%84%E3%81%99)
    - [2.3. リファクタリングしない限り、永久に残る](#23-%E3%83%AA%E3%83%95%E3%82%A1%E3%82%AF%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0%E3%81%97%E3%81%AA%E3%81%84%E9%99%90%E3%82%8A%E6%B0%B8%E4%B9%85%E3%81%AB%E6%AE%8B%E3%82%8B)
    - [2.4. 新たな技術的負債を仲間に呼び出し、増える](#24-%E6%96%B0%E3%81%9F%E3%81%AA%E6%8A%80%E8%A1%93%E7%9A%84%E8%B2%A0%E5%82%B5%E3%82%92%E4%BB%B2%E9%96%93%E3%81%AB%E5%91%BC%E3%81%B3%E5%87%BA%E3%81%97%E5%A2%97%E3%81%88%E3%82%8B)
    - [2.5. 予期せぬデグレードを引き起こす](#25-%E4%BA%88%E6%9C%9F%E3%81%9B%E3%81%AC%E3%83%87%E3%82%B0%E3%83%AC%E3%83%BC%E3%83%89%E3%82%92%E5%BC%95%E3%81%8D%E8%B5%B7%E3%81%93%E3%81%99)
    - [2.6. 読む人の負債への耐性を高めてしまう](#26-%E8%AA%AD%E3%82%80%E4%BA%BA%E3%81%AE%E8%B2%A0%E5%82%B5%E3%81%B8%E3%81%AE%E8%80%90%E6%80%A7%E3%82%92%E9%AB%98%E3%82%81%E3%81%A6%E3%81%97%E3%81%BE%E3%81%86)
    - [2.7. プロジェクトに新規に参加したメンバの成長を阻害する](#27-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AB%E6%96%B0%E8%A6%8F%E3%81%AB%E5%8F%82%E5%8A%A0%E3%81%97%E3%81%9F%E3%83%A1%E3%83%B3%E3%83%90%E3%81%AE%E6%88%90%E9%95%B7%E3%82%92%E9%98%BB%E5%AE%B3%E3%81%99%E3%82%8B)
    - [2.8. 単純なはずの変更にも大量の工数が必要になる](#28-%E5%8D%98%E7%B4%94%E3%81%AA%E3%81%AF%E3%81%9A%E3%81%AE%E5%A4%89%E6%9B%B4%E3%81%AB%E3%82%82%E5%A4%A7%E9%87%8F%E3%81%AE%E5%B7%A5%E6%95%B0%E3%81%8C%E5%BF%85%E8%A6%81%E3%81%AB%E3%81%AA%E3%82%8B)
    - [2.9. 負債を何とかしようとするメンバのモチベーションを下げる](#29-%E8%B2%A0%E5%82%B5%E3%82%92%E4%BD%95%E3%81%A8%E3%81%8B%E3%81%97%E3%82%88%E3%81%86%E3%81%A8%E3%81%99%E3%82%8B%E3%83%A1%E3%83%B3%E3%83%90%E3%81%AE%E3%83%A2%E3%83%81%E3%83%99%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E4%B8%8B%E3%81%92%E3%82%8B)
- [3. 技術的負債との付き合い方](#3-%E6%8A%80%E8%A1%93%E7%9A%84%E8%B2%A0%E5%82%B5%E3%81%A8%E3%81%AE%E4%BB%98%E3%81%8D%E5%90%88%E3%81%84%E6%96%B9)
    - [3.1. 技術的負債という概念の存在を認識する](#31-%E6%8A%80%E8%A1%93%E7%9A%84%E8%B2%A0%E5%82%B5%E3%81%A8%E3%81%84%E3%81%86%E6%A6%82%E5%BF%B5%E3%81%AE%E5%AD%98%E5%9C%A8%E3%82%92%E8%AA%8D%E8%AD%98%E3%81%99%E3%82%8B)
    - [3.2. "自分も負債コードを書いているかも"という意識を持つ](#32-%E8%87%AA%E5%88%86%E3%82%82%E8%B2%A0%E5%82%B5%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E6%9B%B8%E3%81%84%E3%81%A6%E3%81%84%E3%82%8B%E3%81%8B%E3%82%82%E3%81%A8%E3%81%84%E3%81%86%E6%84%8F%E8%AD%98%E3%82%92%E6%8C%81%E3%81%A4)
    - [3.3. "このレビュー対象に負債コードがあるかも"という意識を持つ](#33-%E3%81%93%E3%81%AE%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E5%AF%BE%E8%B1%A1%E3%81%AB%E8%B2%A0%E5%82%B5%E3%82%B3%E3%83%BC%E3%83%89%E3%81%8C%E3%81%82%E3%82%8B%E3%81%8B%E3%82%82%E3%81%A8%E3%81%84%E3%81%86%E6%84%8F%E8%AD%98%E3%82%92%E6%8C%81%E3%81%A4)
    - [3.4. "小さな負債でも放置すると強力な毒になり得る"という意識を持つ](#34-%E5%B0%8F%E3%81%95%E3%81%AA%E8%B2%A0%E5%82%B5%E3%81%A7%E3%82%82%E6%94%BE%E7%BD%AE%E3%81%99%E3%82%8B%E3%81%A8%E5%BC%B7%E5%8A%9B%E3%81%AA%E6%AF%92%E3%81%AB%E3%81%AA%E3%82%8A%E5%BE%97%E3%82%8B%E3%81%A8%E3%81%84%E3%81%86%E6%84%8F%E8%AD%98%E3%82%92%E6%8C%81%E3%81%A4)
    - [3.5. "負債の放置は未来に責任を丸投げする行為に相当する"という意識を持つ](#35-%E8%B2%A0%E5%82%B5%E3%81%AE%E6%94%BE%E7%BD%AE%E3%81%AF%E6%9C%AA%E6%9D%A5%E3%81%AB%E8%B2%AC%E4%BB%BB%E3%82%92%E4%B8%B8%E6%8A%95%E3%81%92%E3%81%99%E3%82%8B%E8%A1%8C%E7%82%BA%E3%81%AB%E7%9B%B8%E5%BD%93%E3%81%99%E3%82%8B%E3%81%A8%E3%81%84%E3%81%86%E6%84%8F%E8%AD%98%E3%82%92%E6%8C%81%E3%81%A4)
- [4. 参考リンク](#4-%E5%8F%82%E8%80%83%E3%83%AA%E3%83%B3%E3%82%AF)

<!-- /TOC -->


# 1. 技術的負債とは何か
- ソフト開発における借金のような存在
- 設計原則に反するあらゆるソースコードが技術的負債と成り得る
- ソフト開発者がコードレビューで指摘し、構成管理への侵入を防ぐべきもの


# 2. 技術的負債の特性・効果

## 2.1. 負債コードは"わからない"
- 以下の2つの"わからない"は、言葉としては同じだが全く別のことを言っている
	1. 言語仕様に疎い設計者が知らない機能を見た時に発する"わからない"
		- 単純に勉強不足なだけで、大抵の場合はインターネット検索で解決できる
	2. 意図を表現できてない負債コードを見た設計者が発する"わからない"
		- 負債コードを書いた本人以外には(時には本人でさえも)説明できない
		- インターネット検索は負債コードの意図は解決してくれない
- 負債コードは後者の方であり、前者は負債ではない(読む人の知識が足りないだけ)

## 2.2. 次の変更に掛かる工数を無駄に増やす
- 意図が伝わりにくく可読性の低いコードを、書いた本人以外が変更しなければならない場合、"書いた本人の気持ちや意図"を推測しながら、理解しながら変更する必要がある
- 意図の無いコードや、書いた本人ですら何故そうしたのか説明できないコードの場合は推測が難しく、安全な変更のために多くの時間を掛けなければならなくなる

## 2.3. リファクタリングしない限り、永久に残る
- 当たり前だが、勝手に負債が解消されることは絶対に無い
- 負債を生み出すのは一瞬だが、生き残った負債は将来そのコードに関わる不特定多数の設計者達を対象として時間を奪い続けてしまう
- 設計者が成長し続けない限り、プロジェクトは技術的負債に直面し続ける可能性がある

## 2.4. 新たな技術的負債を仲間に呼び出し、増える
- 既存の負債コードは(製品に載せるためのレビューに穴があったにも関わらず)"既に製品に載っているから"という理由で正当化される場合があり、いつまでも負債として残り続ける
- 更に、"製品に載っている既存コードは安全である"という思い込みや、"周囲のコードのスタイルに合わせる"という理由から既存の負債コードをコピペしたり、アレンジを加えたりする
- 一度製品に入り込んだ技術的負債は、周囲に影響を与え続け、新たな負債を生み出す要因となる(割れ窓の理論)

## 2.5. 予期せぬデグレードを引き起こす
- 負債コードの意図の読み間違いから思わぬ所でデグレードを起こす場合がある
- 変数や関数の名前からは推測しにくい使い方をしている場合や、1つの変数に意図の無い無機質な名前を付けて複数の用途で使いまわしている場合が当てはまる

## 2.6. 読む人の負債への耐性を高めてしまう
- 負債コードに長く接しすぎると、それが当たり前であるかのように感じてしまう
- 自分が書いたコードやレビュー対象のコードが負債であることを見落とし、レビューの効果が薄れる場合がある
- この`負債コード耐性の高さ`はプロジェクト遂行の上で必要悪となることもあるが、最初から誰でも読めるように書ける方がより評価に値する

## 2.7. プロジェクトに新規に参加したメンバの成長を阻害する
- 入ったばかりで経験の浅い新人は周囲の真似をして勉強する場合が多い
- 新人には真似するコードが負債か否かの判別が付けられず、負債を負債と自覚せずに真似するようになる
- 新人が書いたコードに対して負債を指摘できない人がレビュアだった場合も同様で、新人は"指摘されなかった==正しいコードを書くことができた"と思い込んでしまう
- コードをレビューする立場にある人は、最低でも負債の指摘ができなければならない

## 2.8. 単純なはずの変更にも大量の工数が必要になる
- 技術的負債が持つこれまでの効果から、負債は返済されない限り増え続ける事が分かる
- 負債が積み重なった時、プログラマは極小さな変更であっても細心の注意を払って慎重にコードを書かなければならない状況に追い込まれる
- 1カ所の変更がどこに影響を及ぼすか読み切れない、どこまでテストしておけば安全と言えるのか分からない、という状況での変更になるため、要件からは想像もつかない程時間が掛かってしまう
- 1個ずつの負債がもたらす複雑度の上昇は微々たるものだとしても、それらは掛け算のように作用し合ってプログラムの複雑度を上げ、保守性を最悪のものにしてしまう

## 2.9. 負債を何とかしようとするメンバのモチベーションを下げる
- 負債を返済せずに放置したプロジェクトは遂行困難な状況に直前するリスクを背負っている
- 負債のリスクを知ってるメンバは負債を何とかしようと努力する
	- 自分で書くコードに負債を入れないための努力
	- 自分がレビューするコードから負債を排除する努力
	- 負債について周知し、対策の必要性を説く努力
- その一方で負債の排除を意識できないレビューも数多く存在する

# 3. 技術的負債との付き合い方
- プロジェクトでコードに関わる設計者全員に以下が必要となる

## 3.1. 技術的負債という概念の存在を認識する
- 負債がどのような特性を持ち、放置することで現れる負の効果がどのようなものなのか
- これらを理解していないと、何故対策しなければならないのか、も分からない

## 3.2. "自分も負債コードを書いているかも"という意識を持つ
- どんなに気を付けていても負債コードを書いてしまう可能性はある
- 自分が自信を持っているコードでも、レビューに出す前に疑ってみるべき
- 自分にしか意図の伝わらないコードは`他人にとっては理解が難しい`コードになる
- 負債だらけのコードはレビュアへの負担が大きい、なるべくレビューに出す前に潰しておくべき

## 3.3. "このレビュー対象に負債コードがあるかも"という意識を持つ
- コードを書いた人がどんなにベテランであっても、間違えない保証はどこにも無い
- コードレビューではコードが仕様を満たしているか確認する他に、負債が無いことの確認をするべき
- コードレビューは負債の侵入を防ぐ最後の砦
- レビューに負債を持ち込まないのがベストだが、コードレビューの品質は定期的に見直す方が望ましい

## 3.4. "小さな負債でも放置すると強力な毒になり得る"という意識を持つ
- 個々の負債が持つ毒は小さいことが多く、その脅威は甘く見積もられがち
	- 変数、関数の名前が分かりにくい
	- コメントが間違っている、意図を表現していない
- 脅威の見積もりが甘いと、レビューで見逃されやすくなってしまう
	- "このくらい、別に分かるしいいでしょ"
		- 分かるのは書いた本人だけで、他の人には分からないのでNG
		- もっと最悪なのは書いた本人も説明できないけど動いてるからOK、というパターン
	- "もう設計者評価まで終わってるし、やり直しの工数が無駄だから修正しない"
		- そもそもコードレビューと設計者評価結果レビューをひとまとめにしているのがNG
		- コードレビューをスキップできるのは、負債コードを書かずに変更できる人だけ
		- 負債の指摘が大量に出るようなコードを書くレベルの人は、まずコードレビューを通すべき
	- "// 後で直す、今回は時間が無い"
		- 後で直されることはまず無いのでNG
- 見逃されやすい >> プロジェクトに侵入しやすい >> 数を増やしやすい
	- 小さな負債の組み合わせが強力な毒となるので、1個も見逃してはならない

## 3.5. "負債の放置は未来に責任を丸投げする行為に相当する"という意識を持つ
- 過去に放置された負債の煽りを受けるのは現プロジェクトのメンバである
- つまり、今コードレビューで見逃した負債の煽りを受けるのは未来のメンバである
- 勉強不足や怠慢により生まれた負債の返済義務は、いとも簡単に未来に丸投げされてしまう
	- 負債を返済できない場合は、想定以上にプロジェクトの工数が跳ね上がることになる
- 未来のメンバは自分かも知れない、自分が苦しまないように今頑張るべき
- 未来のメンバが自分でなかったとしても、他人に責任を丸投げするのはモラルが無さすぎるので今頑張るべき

# 4. 参考リンク
- [技術的負債 - Qiita](https://qiita.com/erukiti/items/9cc7850250268582dde7)
- [技術的負債とどうやって戦うか - Qiita](https://qiita.com/kamykn/items/ad687e772da454e3f614)
- [技術的負債への後悔と返済｜timakin｜note](https://note.mu/timakin/n/nf7e2a70905d4)
- [技術的負債という(非エンジニアにとっての)隠しパラメータが生産性100倍を起こす - mizchi's blog](https://mizchi.hatenablog.com/entry/2014/02/19/210801)
- [若手エンジニアを不幸にしないための開発の「べからず」集 - Qiita](https://qiita.com/nonbiri15/items/80e75fbdc358d9144215)
- [若手エンジニアを不幸にしないための開発の「べからず」集 コーディング編 - Qiita](https://qiita.com/nonbiri15/items/6d05e9bfc5ecbdaaab92)
- [若手エンジニアを不幸にしないためのC++コーディングべからず集 - Qiita](https://qiita.com/nonbiri15/items/a6d9176691e6766d2ac3)
- [無理解な職場でのコーディングに関するありがちな発言 - Qiita](https://qiita.com/nonbiri15/items/eddc658067ae415bfb78)

