# chime

毎時0分に現在時刻を macOS の通知で知らせる、最小限の時報 CLI。

```
⏰ 時報
10:00 です
```

## 仕組み

- launchd の `StartCalendarInterval`（毎時0分）で `chime now` を実行する
- 通知は `osascript` の `display notification` で出す（音: Glass）
- スリープ中に過ぎた時報は、復帰直後に1回だけ届く
- 再起動・ログアウト後も自動で動き続ける

## インストール

```sh
chmod +x chime
ln -s "$PWD/chime" ~/bin/chime   # ~/bin が PATH に入っていること
chime start
```

## 使い方

```sh
chime start    # launchd に登録して開始
chime stop     # 登録を外して停止（plist も削除）
chime status   # 動いているか確認
chime now      # 今すぐ1回通知（テスト用）
```

## ファイル

| パス | 内容 |
|---|---|
| `chime` | 本体のスクリプト |
| `~/bin/chime` | 本体へのシンボリックリンク |
| `~/Library/LaunchAgents/com.<ユーザー名>.chime.plist` | `chime start` で生成、`chime stop` で削除 |

本体を編集すると、次の0分から反映される（再登録は不要）。

## 通知が出ないとき

- システム設定 → 通知 で「スクリプトエディタ」の通知を許可する
- 集中モード（おやすみモードなど）がオフか確認する
- `launchctl print gui/$(id -u)/com.$(id -un).chime` で `last exit code` を確認する
