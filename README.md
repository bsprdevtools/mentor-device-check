# mentor-device-check

対面サポート現場でメンターがシニアのスマホにNFCタグをかざす（またはQRコードを読む）と、Chromeが起動し、**機種コード・Androidバージョン・主要アプリ導入リンク**を一画面で表示する静的Webツール。

シニアのスマホの設定アプリを開かずに、機種把握と対象アプリのストア導線が完結する。

公開URL：<https://kazuotoyama.github.io/mentor-device-check/>

## 特徴

- 静的HTMLのみ。サーバー送信・LocalStorage保存・Cookie・外部解析ゼロ
- Chrome on Android の User-Agent Client Hints（`navigator.userAgentData.getHighEntropyValues()`）で機種コードとOSバージョンを取得
- 表示する情報は画面に描画するだけ、どこにも保存・送信しない
- HTTPS必須（GitHub Pagesで配信）

## 仕組み

1. NFCタグ（NTAG213推奨）またはQRコードに本ツールのURLを書き込む
2. シニアのスマホにかざす → Chromeが本ページを開く
3. ページ側で機種・OS情報を取得・表示
4. メンターがその情報を元に、対象アプリのストア導線を案内する

## 表示される項目

| 項目 | 例 | 取得元 |
|---|---|---|
| 機種コード | `SCG29` | `Sec-CH-UA-Model` |
| Androidバージョン | `15` | `Sec-CH-UA-Platform-Version` |
| 推定SDK | `API 35` | バージョンから推定 |
| ヘルスコネクト | OS統合済み / インストール必要 / 非対応 | SDKレベルで分岐 |

## NFCタグ運用

- 推奨タグ：NTAG213（容量144byte、1枚30〜170円）
- 推奨アプリ：NFC Tools（Android, 無料）
- 書き込み後、必ず **NDEFロック** を実施（タグ改ざん防止）

## ローカル動作確認

```sh
git clone https://github.com/KazuoToyama/mentor-device-check.git
cd mentor-device-check
open index.html
```

実機検証はGitHub Pages等のHTTPS環境にホスティングしてから行う（UA Client HintsはHTTPS必須）。

`?debug=1` をURLに付けるとデスクトップでも検証用表示が出る。

## プライバシー

- 取得情報：機種コード・Androidバージョン・モバイル判定
- 送信先：**なし**（端末ローカルで描画するだけ）
- 保存：**なし**（LocalStorage/Cookie/IndexedDB 不使用）
- 解析ツール：**なし**（Google Analytics等は組み込んでいない）

詳細は [プライバシーポリシー](https://kazuotoyama.github.io/legal-docs/mentor-device-check/privacy/) を参照。

## ライセンス

MIT License — [LICENSE](./LICENSE) を参照。

## トラブル

- 「機種コードが取得できませんでした」：Chrome以外で開いた場合。Chromeで開き直すか、画面の「手動で入力する」ボタンから入力可能
- iPhoneで開いた：iOS SafariはUser-Agent Client Hints非対応のため、専用の案内画面を表示
- Android 9未満：ヘルスコネクト非対応の機種である旨を表示

## バージョン

- v1.0.0（2026-05-16）：初版リリース
