# soiya59.github.io

「おやこポイント」のNFCタグから、ブラウザではなくアプリが開くようにするための
リンク設定だけを置いているリポジトリです。表示するページはありません。

## 置いてあるもの

| ファイル | 役割 |
|---|---|
| `.well-known/assetlinks.json` | Android App Links。このドメインのURLを `jp.soiyalab.oyakopoint` が開いてよい、という宣言 |
| `.well-known/apple-app-site-association` | iOS Universal Links。同じ宣言のiOS版（拡張子なしが正しい） |
| `.nojekyll` | GitHub Pagesが `.` で始まるフォルダを無視しないようにするための空ファイル |

## なぜこのリポジトリが要るのか

アプリ本体のWeb版は `soiya59.github.io/oyakopoint/` にあるが、
**この2ファイルはドメインの直下（`soiya59.github.io/.well-known/`）でなければ効かない。**
`/oyakopoint/` の下に置いても無視される。そのため、ドメイン直下を担当する
ユーザーページ用リポジトリ（`<ユーザー名>.github.io` という名前）を別に用意している。

## 中の値について

`assetlinks.json` のSHA-256は2つ入れてある。

- 1つ目: **アプリ署名鍵**。Google Playが配信時に再署名するときの鍵
- 2つ目: **アップロード鍵**。EAS Buildが作った鍵

Playストア経由で入れた場合とEASから直接入れた場合で署名が変わるため、両方必要。
取得方法は `oyakopoint/開発部/成果物/実装メモ.md` を参照（Play Consoleの画面からは
見つけられず、署名済みAPKとAABから `apksigner` / `keytool` で読み出した）。

`apple-app-site-association` の `appID` は `<Team ID>.<Bundle ID>` の形式。

## 変更したとき

**アプリのビルドはやり直さなくてよい。**このファイルはアプリ起動時ではなく、
OSがドメインを検証するときに読まれる。ただし反映には時間がかかることがあり、
Androidは再インストールか `adb shell pm verify-app-links` での再検証が要る場合がある。
