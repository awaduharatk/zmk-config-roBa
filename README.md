# roBa

## 設定方法

- [keymap-editor](https://nickcoutsos.github.io/keymap-editor/)

## キーボードへの適用方法

1. `config/roBa.keymap` などの変更を `git push` する
2. GitHub Actions の `Build ZMK firmware` ワークフローが自動実行されるのを待つ
3. 完了したら Actions ページの最新 run を開き、`firmware` Artifact をダウンロード
4. zip を展開すると以下の uf2 が入っている
   - `seeeduino_xiao_ble-roBa_L-zmk.uf2` (左手用)
   - `seeeduino_xiao_ble-roBa_R-zmk.uf2` (右手用)
   - `seeeduino_xiao_ble-settings_reset-zmk.uf2` (設定リセット用、必要時のみ)
5. 書き込みは **右 (Central) → 左 (Peripheral) の順** で実施（右が USB 接続側 / Central のため、先に起動させた方がペアリングが安定する）
   1. キーボードの XIAO BLE のリセットボタンを **2回素早く押す** とブートローダーモードになり、PC に `XIAO-SENSE` などの USB ストレージとしてマウントされる
   2. 対応する uf2 ファイルをドラッグ&ドロップでコピー
   3. 自動で再起動して反映される
6. 右が終わったら左も同じ手順で書き込んで完了

> ペアリング情報がおかしくなった場合は左右両方に `settings_reset` の uf2 を書き込み、その後 右 → 左 の順で通常ファームを書き直す。
