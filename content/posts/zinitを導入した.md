---
title: "Zinitを導入した"
date: 2021-12-14T19:03:50-08:00
---

macOSでのデフォルトのシェルがzshになって久しいですが、それに合わせて`.zshrc`を適当運用してきたことのつけがzshの起動速度に影響してきたのでどうにかしようとした記録です。

# zinitとは
zsh用のプラグインマネージャーであり、zshの高速化ができるらしいという話を聞いたので試してみることにしました。自分は主に
- https://zenn.dev/xeres/articles/2021-05-05-understanding-zinit-syntax
- https://watiko.net/2021/12/05/renew-zshrc/
の記事とGitHubの[README](https://github.com/zdharma-continuum/zinit/blob/main/README.md)をみながら進めました。

# 計測
`hyperfine 'zsh -i -l -c exit'`で計測してみたところ起動に約2秒かかっていることがわかりました。次に[この記事](https://watiko.net/2021/12/05/renew-zshrc/)で紹介されている`zprof`の機能を使って関数毎にかかっている時間を調べました。その結果、補完機能関連の部分に多く時間がかかっていることがわかりました。

# zsh-users/zsh-completionsのインストール方法の変更
まず自分はローカルにインストールされているcompletionのファイルをzinitが、見に行くように設定しましたが、それだと補完がうまく効くコマンドと効かないコマンドが出てきてしまいました。その原因は自分がzsh-completionのディレクトリに関係ないサードパーティの補完ファイルを入れていることでした。なのでそのサードパーティの補完ファイルを`site-functions/`以下に移動させた後、Homebrew経由でインストールされていたzsh-users/zsh-completionsをzinit経由でのインストールに切り替え遅延ロードすることにより、起動時間の短縮を達成しました。

# 結果
`hyperfine 'zsh -i -l -c exit'`でもう一度計測したところ、1秒を切るくらいの時間で起動できるようになっており、無事に高速化に成功したようです。

# 参考
- https://watiko.net/2021/12/05/renew-zshrc/
- https://unix.stackexchange.com/questions/26555/fpath-in-zsh-functions-and-site-functions
- https://zenn.dev/xeres/articles/2021-05-05-understanding-zinit-syntax