---
title: "ミクシィでインターンした 2022"
date: 2022-11-27T06:10:49Z
tags:
    - swift
    - mixi
draft: true
---

昨年に続いて株式会社MIXIにて**Dive into mixi GROUP**というインターンシップでmixiグループの[minimo](https://minimodel.jp)事業部にてiOSエンジニアとしてインターンさせていただきました。

## tl;dr

みなさんも[応募](https://mixigroup-recruit.mixi.co.jp/recruitment-category/internship/9575/)しましょう！

## 選考について

昨年お世話になった人事の方から今年の夏のインターンにお誘いいただき、自分としても昨年非常に良い経験ができたと感じていたので、今年も応募することにしました。昨年のプロセスと同様に、最初に人事の方との面接、その後エンジニアの方との面接を経て今年のインターンにも合格をいただきました。

## 待遇、環境、期間等

昨年と同様の待遇で、期間についても全面的にこちらの希望に沿った形にしていただきました。今年は日程のほとんどをオフィスでの勤務ができたので昨年より多くのコミュニケーションを取ることができたのはよかったかなと思いました。

## インターン期間中に取り組んだこと

### メニュー新規作成時に、任意で画像を追加できるようにする

最初に取り組ませてもらったタスクは機能改善形のタスクでした。minimoでは美容師（やその他施術者）のユーザーがメニューを登録するステップがあるのですが、メニュー新規登録時に画像を追加する項目がないということが問題として挙げられておりそれの改善を行いました。この機能の経緯としては以前は必須項目としてあったが掲載のハードルを下げるために無くしたということでしたが、メニューに画像がある状態のほうがお客様は予約しやすいという判断がされてその修正を行いました。修正前では、画像を追加したい場合は、「新規作成」→「作成したメニューを編集」の手順を踏む必要があり、手間がかかるということを変更しました。

最終的におこなった変更自体はとても小さなものになりましたが、作業をする中でデザインファイルと実際の実装との差異を見つけたり、仕様の詳細について確認する必要があったりと、業務の進め方について学びが多いタスクになりました。

### SwiftLintルールの改善

minimoではコード品質の維持のためにSwiftLintを利用していました。そこで新たに役にたつと思われるルールを追加しました。追加したのは以下のルールです。

- [explicit_init](https://realm.github.io/SwiftLint/explicit_init.html)
- [trailing_closure](https://realm.github.io/SwiftLint/trailing_closure.html)
- [redundant_nil_coalescing](https://realm.github.io/SwiftLint/redundant_nil_coalescing.html)
- [cyclomatic_complexity(ignores_case_statements = true)](https://realm.github.io/SwiftLint/cyclomatic_complexity.html)
- [xct_specific_matcher](https://realm.github.io/SwiftLint/xct_specific_matcher.html)

これらのタスクを通してSwiftLintへのコントリビューションをすることもでき、面白い経験ができたタスクとなりました。

### ビルド時間計測システム構築

iOSアプリのビルド時間を継続的に監視する仕組みの構築を行いました。ビルド時間の増大は開発者の体験を損ねる要素の一つであり、ビルド時間に影響を与えうる変更をいくつか自分で行なっていたこともあり、継続的に計測する仕組みを用意することには意味があると考えてこのタスクに取り組まさせていただきました。簡単に仕組みを説明するとBitriseのワークフローの定期実行機能により、GitHub上にあるコードに対してfastlaneの独自laneを実行します。そのlaneの中ではhyperfineというコマンドを使いxcodebuildがアプリをビルドするのにかかる時間を計測します。そしてその結果をJSONにエクスポートしAWS S3のバケットに保存します。S3に保存されたJSONはrundeckという別のサービスを経由してBigQueryに送られます。最終的にLookerというツールを利用しビジュアライズしました。

このタスクでは、今まで自分が利用したことのない技術を扱うことができ、とても勉強になりました。また多くの社員の方に助けていただき、いろいろな方とコミュニケーションを取りながら進められたのでよかったです。

### Swift Package Manager自動アップデートワークフロー作成

CocoaPods, RubyGemsでは同様の仕組みを運用していたので、Swift Package Managerで管理しているライブラリも同様の仕組みを利用していきたいという動機がありました。またライブラリの管理をSwift Package Managerにできるだけ移行していきたいこともあり将来的には必要になることがわかっていました。
この仕組みを実現する上でXcode内蔵SPMを利用しなければいけないことがトリッキーでしたが`xcodebuild -resolvePackageDependencies`をうまく利用することで想定していた動作を実現することができました。最終的にfastlaneのカスタムlaneをBiriseから定期実行して利用します。

### リファクタリング

- コンパイル時間が長いところのリファクタ
  - 複数の変数に分ける
  - 型情報の追加
- マジックナンバーの定数化
- ローカルでは.swiftファイルが変更された時のみSwiftLintを実行
  - Build Phase Script内で.swiftのファイルの変更時のみ SwiftLintを実行するように変更

## 感想