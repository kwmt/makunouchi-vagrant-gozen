# gozen

## ローカル環境構築

ローカル環境はVagrantで構築しています。


```
% git clone git@github.com:kwmt/makunouchi-vagrant-gozen.git
% cd makunouchi-vagrant-gozen
% vagrant up
```

無事 vagrant が立ち上がったら vagrant 内で ansible を実行してください

```
vagrant ssh -c "ansible-playbook -i ~/provision/hosts ~/provision/playbook.yml"
```

vagrantとローカルファイルシステムとのマウントが失敗する場合は vagrant 側のkernelを更新してから再実行してください

```
vagrant ssh  -c "sudo yum update -y"
```

```
vagrant reload
```



## IntelliJ 開発手順

マニュアル
https://github.com/go-lang-plugin-org/go-lang-idea-plugin/wiki/v1.0.0-Setup-initial-project

### 設定

SDKは gozen/golang/go-1.6 を指定してください

Preferences -> Language & Frameworks -> Go -> Go Libraries 

Global libraies にプロジェクトRootを設定

Project libraies に src/gozen/vendor を設定してください

src/gozen/vendor 以下にシンボリックリンクを作成してください

```
ln -s ./ ./src
```

※IntelliJのGo plugin がまだ vendor に対応してないっぽいので。

※glideでインストールしたライブラリの補完が効かない時は、ホスト側で下記を実行してください。

```
% cd gozen(このプロジェクトのパス)/src/gozen
% ./symlinkVendor.sh
```

glide.yamlを変更したら(ライブラリを追加したら)、実行しなおしてください。

### Goをビルド&実行するには

```
$ vagant ssh
$ cd /var/work/src/gozen
// フォーマット・ビルド・実行する
$ ./build.sh
```

curlなどでアクセスしてレスポンスを確認

```
$ curl 'http://172.16.17.10/sample'
```



### mysqlに接続するには

```
$ mysql -h 127.0.0.1  -u gozen -D gozen -p
```

### 依存パッケージを更新するには

```
$ vagant ssh
$ cd /var/work/src/gozen
$ glide up
```

### マイグレートするには

```
$ vagant ssh
$ cd /var/work/src/gozen
$ goose up
```
参考: https://bitbucket.org/liamstask/goose/

### 環境変数の設定

DEVTAB_ENV に

- 本番 production
- ステージング staging
- ローカル development

を設定することで設定等が環境に合わせた設定になります

### 設定ファイルについて

設定ファイルに書くことで、本番(production)・ステージング(staging)・ローカル(development)の環境設定を切り替えることができます。

設定ファイルの場所は下記にあります。

```
/var/work/src/gozen/config/environment/
```

下記のようなファイル名規則になっています。`<環境名>`には `production`・`staging`・`development`が入ります。

```
conf.<環境名>.yml
```


### コーディング規約

log出力には、Go標準の`log`を使用せず、 `github.com/google/logger`を使用しましょう。
