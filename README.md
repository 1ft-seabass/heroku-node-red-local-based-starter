# heroku-nodered-local-based-starter

## 詳細記事

- [ローカルの作業データを元に Heroku で動かす Node-RED の仕組みを GitHub に公開したメモ – 1ft-seabass.jp.MEMO](https://www.1ft-seabass.jp/memo/2021/10/09/made-heroku-node-red-local-based-starter/)

## ざっくり、以下の対応をしています

- Git の初期設定 main ブランチで作業できます
- 最低限の .gitignore ファイル設定済み
- プロジェクトフォルダ直下の Node-RED 設定データ集約フォルダ作成済み
- 設定ファイルは Node-RED 設定データ集約フォルダ内の setting.js を作成済み
-  `/api1` で簡単なレスポンスを返す HTTP API なフローを作成済み
- Heroku Procfile ファイルの設定を作成済み
- setting.js の httpAdminRoot  設定を `/admin` に設定済み
- setting.js の adminAuth を設定してログインを Heroku の環境変数で設定している NODERED_LOGIN_ID と NODERED_LOGIN_PASSWORD でログインできるように設定済み
- 手元の Node-RED で追加インストールが反映されるように Heroku Procfile + package.json 調整済み
- setting.js の `httpStatic: './node-red-static/',` に設定して `node-red-static` をフロントエンドまわりのファイルを置いて公開できる場所を設定済み
- Node-RED 設定データ集約フォルダを node-red_dir フォルダにしていたが、リネームして .node-red でやってみることにした

## 更新するときのおおまかな作業フロー

この仕組みは、ローカルで作業データを元に Heroku で動かす Node-RED なので、Heroku の仕組みを更新する場合は以下のように対応していきます。

- ローカルで `npm run node-red`でローカルの Node-RED 起動
- `http://127.0.0.1:18801/admin/` にブラウザからアクセスして作業。
- ローカルで Node-RED のフローをつくって、Heroku に反映していいなってところで落ち着かす。
- ローカルで node-red-static フォルダでフロントエンドな作業をして、Heroku に反映していいなってところで落ち着かす。
- ローカルで Node-RED で追加したいノードがあればインストールして作業をつづけ、Heroku に反映していいなってところで落ち着かす。
- ローカルで変更内容をインデックスに追加してコミット対象にする
    - いわゆる `git add` と `git commit`
- `git push heroku main` で Heroku に反映
- Heroku に反映されたら動作確認
    - `https://<今回のHerokuアドレス>/` でフロントエンドまわりの確認
    - `https://<今回のHerokuアドレス>/admin/`で Node-RED のフローが今回の反映ぶん動いているか確認
- Heroku でのみおかしい Node-RED の挙動があれば Heroku 側のフローに debug ノード入れたりデバッグする
    - この変更は、再度ローカルから新しい更新をすると以前のものに戻るので注意
    - なにかしらフローで解決したら、いったんローカルに戻ってローカルの Node-RED のフローに同じ変更をして、次回反映
- 以降は、作業をローカルに戻って繰り返す

## 以降の更新履歴

- npm run コマンドの node-red をシンプルな node-red 呼び出しからはじめました。
  - `./node_modules/.bin/node-red` だと動かなくなったので調整
  - ちゃんと `npx node-red` 的に動くようで、問題なし