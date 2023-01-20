---
title: MIXIでインターンした話 in 2022
date: 2023-01-20T21:51:09.929Z
tags:
  - MIXI
  - swift
  - internship
draft: false
keywords:
  - internship
  - ios
  - swift
  - MIXI
slug: internship-at-mixi-in-2022
---

昨年に続いて株式会社MIXIにて**Dive into MIXI GROUP**というインターンシップでMIXIグループの[minimo](https://minimodel.jp)事業部にてiOSエンジニアとしてインターンさせていただきました。

<a href="https://apps.apple.com/jp/app/minimo-%E3%83%9F%E3%83%8B%E3%83%A2-24%E6%99%82%E9%96%93%E4%BA%88%E7%B4%84%E5%8F%AF-%E7%BE%8E%E5%AE%B9%E3%82%B5%E3%83%AD%E3%83%B3%E4%BA%88%E7%B4%84%E3%82%A2%E3%83%97%E3%83%AA/id719858778?itscg=30200&amp;itsct=apps_box_appicon" style="width: 170px; height: 170px; border-radius: 22%; overflow: hidden; display: inline-block; vertical-align: middle;"><img src="https://is1-ssl.mzstatic.com/image/thumb/Purple123/v4/28/df/17/28df1703-b6bd-db1c-dd52-e7a747ae4213/AppIcon-1x_U007emarketing-0-7-0-85-220.png/540x540bb.jpg" alt="minimo（ミニモ）24時間予約可！美容サロン予約アプリ" style="width: 170px; height: 170px; border-radius: 22%; overflow: hidden; display: inline-block; vertical-align: middle;"></a>

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

### Swift Package Manager自動アップデートワークフロー作成

CocoaPods, RubyGemsでは同様の仕組みを運用していたので、Swift Package Managerで管理しているライブラリも同様の仕組みを利用していきたいという動機がありました。またライブラリの管理をSwift Package Managerにできるだけ移行していきたいこともあり将来的には必要になることがわかっていました。
この仕組みを実現する上でXcode内蔵SPMを利用しなければいけないことがトリッキーでしたが`xcodebuild -resolvePackageDependencies`をうまく利用することで想定していた動作を実現することができました。最終的にfastlaneのカスタムlaneをBiriseから定期実行して利用します。

### SwiftLintルールの改善

minimoではコード品質の維持のためにSwiftLintを利用していました。そこで新たに役にたつと思われるルールを追加しました。追加したのは以下のルールです。

- [explicit_init](https://realm.github.io/SwiftLint/explicit_init.html)
- [trailing_closure](https://realm.github.io/SwiftLint/trailing_closure.html)
- [redundant_nil_coalescing](https://realm.github.io/SwiftLint/redundant_nil_coalescing.html)
- [cyclomatic_complexity(ignores_case_statements = true)](https://realm.github.io/SwiftLint/cyclomatic_complexity.html)
- [xct_specific_matcher](https://realm.github.io/SwiftLint/xct_specific_matcher.html)

SwiftLintの自動フォーマット機能を利用することで比較的簡単にルールを適用することができました。またこれらのタスクを通してSwiftLintへのコントリビューションをすることもでき、面白い経験ができたタスクとなりました。

### ビルド時間計測システム構築

iOSアプリのビルド時間を継続的に監視する仕組みの構築を行いました。ビルド時間の増大は開発者の体験を損ねる要素の一つであり、ビルド時間に影響を与えうる変更をいくつか自分で行なっていたこともあり、継続的に計測する仕組みを用意することには意味があると考えてこのタスクに取り組まさせていただきました。簡単に仕組みを説明するとBitriseのワークフローの定期実行機能により、GitHub上にあるコードに対してfastlaneの独自laneを実行します。そのlaneの中ではhyperfineというコマンドを使いxcodebuildがアプリをビルドするのにかかる時間を計測します。そしてその結果をJSONにエクスポートしAWS S3のバケットに保存します。S3に保存されたJSONはrundeckという別のサービスを経由してBigQueryに送られます。最終的にLookerというツールを利用しビジュアライズしました。

このタスクでは、今まで自分が利用したことのない技術を扱うことができ、とても勉強になりました。また多くの社員の方に助けていただき、いろいろな方とコミュニケーションを取りながら進められたのでよかったです。

### リファクタリング

- コンパイル時間が長いところのリファクタ
  - 複数の変数に分ける
  - 型情報の追加
- マジックナンバーの定数化
- ローカルでは.swiftファイルが変更された時のみSwiftLintを実行
  - Build Phase Script内で.swiftのファイルの変更時のみ SwiftLintを実行するように変更

## 感想

今回のインターンでは開発環境の改善に関連したタスクを色々とやることができ新しい経験を得ることができてとても面白かったです。一年ぶりにコードベースを見返すと違った視点で見れることも多く、それもまた面白い経験でした。また去年インターンさせていただいた時よりも多くの方々とコミュニケーションを取ることができたと思うのでよかったです。
