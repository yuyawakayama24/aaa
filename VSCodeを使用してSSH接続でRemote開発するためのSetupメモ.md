# VSCodeを使用してSSH接続でRemote開発するためのSetupメモ

## TL;DR

Tera Termからの操作がつらかったので自端末のVSCodeから開発作業できるようにした

## Remote Development

### Remote Developmentとは

Microsoft謹製リモート開発用拡張パック。  
2019/6/28現在、Preview。

### Remote Developmentインスコ事前準備

#### Windows7の場合

インストールされる拡張機能「Remote - SSH」がSSHクライアントを使うために  
[Git for Windows](https://gitforwindows.org/)をインストールする必要がある。  
また、VSCodeがSSHクライアントの場所を判断するため、環境変数PATHに下記を追加。

```path
C:\Program Files\Git\usr\bin
```

#### Windows10の場合

OpenSSHをインスコする必要があるらしい。（試していないので詳しくはわからない）

### Remote DevelopmentのインスコとRemote - SSHのセットアップ

- VSCode拡張機能で「Remote Development」を検索してインスコ。

- 「C:\Users\ユーザー名\.ssh\config」にSSH接続するときの情報を追加

  ```config
  Host [名前（わかりやすい名前をつける）]
      HostName [ホストのIP]
      Port 22
      User centos
      IdentityFile \\10.147.210.19\commonlib\zeafamily\04.インフラGrp\インフラ検証環境\key\matchingtest_20170126.pem
      IdentitiesOnly yes
      ServerAliveInterval 60
      TCPKeepAlive yes
      PasswordAuthentication no
  ```

- 「Connect to Host in Current Window」をクリックして接続してみる

- 接続できたらOK（その際、リモート側にvscode-server?（忘れた）のようなものがインストールされる）

- end

### Remote接続したときの拡張機能について

一部の拡張機能（GitLens, Git History, etc...）はローカルにインストールされていても動かないので
使いたい場合はリモート側にインストールする必要がある。

## GitLens

### GitLensとは

ブランチきったり、コミットしたりをGUIベースでできる拡張機能。  
VSCodeに入っているやつは使いにくい。

### GitLensのインスコ

- VSCode拡張機能で「GitLens」を検索してインスコ。
- end

### gitignore

`/home/centos/.gitignore`に下記追記

```.gitignore
.vscode
```

## トラブルシューティング

### SSH接続で失敗する

リモートサーバーで`vscode-server`なるものが複数立ち上がっているとなぜかわからないが失敗するときがある。
接続リトライ中に`ps afux`で`vscode-server`のプロセスを探して、`kill`コマンドでプロセスをkillすればうまくいくはず。
