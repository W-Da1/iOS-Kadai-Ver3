# iOS-Kadai-ver3

## 概要

本プロジェクトは株式会社ゆめみ様が、iOS エンジニアを希望する方に出す課題のベースプロジェクトです。インターン申込再挑戦のため、概要を詳しく読んだ上で課題に取り組みました．

## アプリ仕様

本アプリは GitHub のリポジトリーを検索するアプリです。

### 環境

- IDE：Version 13.4.1 (13F100)
- Swift：Apple Swift version 5.6.1
- 開発ターゲット：iOS 15.5

### 動作

1. 何かしらのキーワードを入力
2. GitHub API（`search/repositories`）でリポジトリーを検索し、結果一覧を概要（リポジトリ名）で表示
3. 特定の結果を選択したら、該当リポジトリの詳細（リポジトリ名、オーナーアイコン、プロジェクト言語、Star 数、Watcher 数、Fork 数、Issue 数）を表示

## 前回の挑戦で取り組んだ課題
### 1.ソースの可読性向上
- 命名規約（参考：Swift API Design Guidelines）

[`参考1`](https://qiita.com/fuwamaki/items/f2df71723ab277dffc29) :Swiftの命名規則を理解する（Swift API Design Guidelines - Naming 日本語まとめ）

[`参考2`](https://www.swift.org/documentation/api-design-guidelines/#naming) :swift.org（Naming）
をみながら対策を行なった．


- ネスト

[`参考1`](https://techblog.recochoku.jp/8058) :【Swift】安全にアンラップするために 〜!（強制アンラップ）とif letとguard letと??（Nil coalescing operator）の使い分け〜
guard let ~ else {return} を使用し，if文による深いネストを極力避けた．


- インデント
    対策を行なった．
- コメントの適切性
    関数としてまとめた機能含め，わかりにくい動作の補足を行なった．
- スペースや改行
    書き方を統一した．
- その他
    処理を関数としてまとめ，Viewの動作を行う処理が長くなってしまわないよう対処した．


### 2.ソースコードの安全性の向上
guard let ~ else {return} を使用し，以下の項目に対応した．
- 強制アンラップ
- 強制ダウンキャスト
- 不必要なIUO
- 想定外の nil の握り潰し

[`参考1`](https://techblog.recochoku.jp/8058) :【Swift】安全にアンラップするために 〜!（強制アンラップ）とif letとguard letと??（Nil coalescing operator）の使い分け〜

### 3.バグを修正
- レイアウトエラー
stackviewの位置が定まっていなかったので修正.
- メモリリーク
クロージャによる循環参照が原因でメモリリークが発生するようです．以下の記事を参考に[weak self]追加で対処．
[`参考1`](https://tm-progapp.hatenablog.com/entry/2021/01/21/215819) : Swiftで循環参照によるメモリリークを起こしてしまった話
- パースエラー
ViewController2.swiftにて,閲覧者数を取得するキーワード"wathcer_count"がtypo.修正済みだが，APIの仕様変更により，現在このキーワードで取得できるのはスター数とのことでした．

### 4.FatVC
GitAPIの処理がViewController内に記述されていたので，GithubData.swiftへの切り出しを行い対処

### 5.その他
ViewController.swift,ViewController2.swift→SearchViewController.swift, DetailViewController.swiftに名前変更

## 以下今回の改善点及びアピールポイント
### 6.前回の改善点への対応
 - urlSessionTaskのresumeとreloadDataの切り離し(GithubDataへの切り離し)のバグ→ NotificationCenterの導入により，バグを解決
 -  UISearchBarの表示テキスト→placeholderを使用し設定
 -  文字列や配列の空チェック→countでなく、isEmptyを使用
 -  DetailViewControllerがSearchViewControllerの参照を持っているが不要→見直したところその通りでありました．参照削除済み
 -  Cell→dequeueReusableCellを使用することでCellの再利用を行えるよう変更
 -  ダークモードへの対応→[`こちら`](https://qiita.com/gonsee/items/c04b73787730c0e831df)を参考にdynamicColorを導入し，動的にラベルの色を変更
 -  小さい端末や横画面時に詳細画面の表示ができない→UIScrollView及びAuto Layoutによりシミュレータ上最も小さな端末(iPhone8,SE)から最も大きな端末(iPad Pro 12.9inch(5generation))まで全てにおいて動作するように改善，横画面表示も対応([`参考1`](https://swallow-incubate.com/archives/blog/20200805))([`参考2`](https://qiita.com/ynakaDream/items/960899183c38949c2ab0))([`参考3`](https://type.jp/et/feature/3112/))([`参考4`](https://developer.apple.com/documentation/uikit/uiscrollview))
 - 以上，前回指摘していただいた全ての改善点に対応しました．

### 7.オリジナリティ
 - UXを意識し，全ての動作を縦スクロールのみで完結するよう「Present Modally」による画面遷移に変更しました．

### 8.今後の課題
 - GithubDataのユニットテストの導入
 - GithubDataに対するprotocolを定義し，スタブを作成した上でUIテストを導入
 - MVCなどのアーキテクチャの導入

### 9.今回の再学習にあたっての参考文献
[`Swift実践入門`](https://gihyo.jp/book/2020/978-4-297-11213-4)
[`iOSアプリ設計パターン入門`](https://peaks.cc/books/iOS_architecture)

### 10.コメント
時間はかかってしまいましたが，本課題を通じてSwift/Xcodeに大分慣れることができました．今回は絶対にコードレビューに受かるぞ！という気持ちで，前回いただいたコードレビューを参考に一生懸命制作いたしました．伸び代には自信があります！宜しくお願い致します！

### 11.フィードバック
ーーーーーーーーーーーーーーーーーーーー
## 課題完成度
- [ ]：未達成
- [/]：一部達成
- [x]：達成

### 初級
- [/]  可読性の向上
- [/]  安全性の向上
- [x]  バグの修正
- [/]  Fat VCの回避

### 中級
- [ ]  構造のリファクタリング
- [ ]  アーキテクチャの適用
- [ ]  テストの追加

### ボーナス
- [ ]  UIのブラッシュアップ
- [ ]  機能の追加

## 良かった点
- 端末の回転やダークモードに対応している
- README.md が充実している
- いくらか触ってもアプリがクラッシュしないのは良いです

## 改善点
- Git のコミット粒度が大き過ぎです

Git は作業内容の確認や、不具合があった場合に変更を取り消すなど、ソースコードを適切に管理するものです。
また、採用関連に絡むと、どのような工程で改良したのかが分からないので良くないです。

- テストが不十分です

取得したデータを評価していないので、テストとして機能していないです。
コメントでデータが取得できないとありますが、これはテスト対象が非同期処理に対して、
完了するまでの「待ち」の処理がないためです。

また、そのままAPIをリクエストをしているので、
結果はネットワーク状態やAPIサーバー側に左右されるため、アプリ側のテストとして、良くないです。
リクエスト部分をモック化して、正常・異常系を開発側で制御できるようにすると良いです。

- 日本語で検索できません。エンコードして渡すとよいです。

- エラー処理が不十分です。

キーワードに日本語を与えたり、ネットワークがオフラインの場合に、アプリが何もしないです。
クラッシュしない今の処理は素晴らしいのですが、何かしらユーザーに状態を伝えるようにした方が親切です。

- 画面レイアウトが不十分です

詳細画面のレポジトリ名や「Written in XXX」で、文字列が長い場合に正しく表示されていません。
また、詳細画面の下部に余白が設定されています。これはデザインとは判定しずらいです。

- GithubData について

前回他の人がレビューしたので、レビュー内容を把握していないのですが、まだ不十分です。
アプリ全体に対して汎用的になっていて大きすぎるので、目的や機能ごとに分けて設計すると良いです。

レポジトリ取得時に urlSessionTaskOfGithubData を生成してパブリック変数にして
実際の制御や操作は SearchViewController　に任せているように見えます。
責務分けとして、ViewController は何の手段でリクエストしているかは重要ではなく、
データ取得のリクエストと取得されたことが分かれば良いので、
urlSessionTaskOfGithubData　は　GithubData　に内包して、
ViewController　からはシンプルなメソッドで呼び出しできるようにすると、
責務分けがスッキリすると思います。

DetailViewController　を表示するとき、GithubData　を与えていますが、
表示に不要なデータも渡しているので、よくないです。
必要なレポジトリのデータだけを渡すようにすると良いです。
また、渡す前に touchedCellIndex に値を代入しないといけないのは、事故を誘発しそうです。

## その他

今回は再挑戦してもらい、ありがとうございます！
しかしながら、まだ修正箇所がいつくか見受けられました。
いったんゆめみ課題に捉われずに、他のチュートリアルを試して、知識の幅を広げると良いかもしれません。

iOS App Dev Tutorials
https://developer.apple.com/tutorials/app-dev-training


- レポジトリのデータ扱いについて

JSONデータから辞書型に変換して使用していますが、その key 名を Xcode で補完できないため、
人為的ミスを誘発する可能性があります。また、例えば、データを操作して表示したい場合に、
辞書型のままでは好ましくない場合があります。
標準で JSON からクラスを生成する　JSONDecoder　が用意されているので、それを利用すると良いです。
　
https://developer.apple.com/documentation/foundation/jsondecoder

- print について

各所に print() がありますが、開発中は問題ないですが、ソースコード公開やリリース時には不要です。
printのたびにコンソールログに表示されるので、リリース時に思いがけない負荷になったり、
本番アプリの大切な情報を他者が見てしまう危険性があります。
開発中だけ表示する print などを使うと良いです。いろいろやり方はありますが、一例のリンクを貼っておきます。

https://zenn.dev/kyome/articles/4c1ee00c139da3
ーーーーーーーーーーーーーーーーーーーー
