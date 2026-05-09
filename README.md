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

## IME ON / OFF (Windows + US配列)

キーボード側のコンボ：

| コンボ       | 物理キー | 送信キー   | 動作         |
| ------------ | -------- | ---------- | ------------ |
| `muhenkan`   | S+D      | F13        | IME OFF      |
| `henkan`     | D+F      | F14        | IME ON       |
| `ime_toggle` | F+G      | Ctrl+Space | 従来のトグル |

US配列では `INT_HENKAN`/`INT_MUHENKAN` のキーコードが Windows のキーボードドライバ層でフィルタされ IME に届かないため、F13/F14 を中継キーとして使い、**PowerToys で「変換 / 無変換」VK に再マップ → Microsoft IME で「変換 / 無変換」を IME-オン / IME-オフ に割り当て** という二段構成にしている。

### セットアップ 1：PowerToys Keyboard Manager

PowerToys が **常駐していないと F13/F14 の再マップが効かない**。別 PC を使う場合はそちらにも同じ設定が必要。

1. [Microsoft PowerToys](https://learn.microsoft.com/ja-jp/windows/powertoys/) をインストール
2. PowerToys を起動 → **Keyboard Manager** を有効化
3. **キーの再マップ** を開き、以下を追加：

   | 物理キー (送信側) | マップ先        |
   | ----------------- | --------------- |
   | F13               | IME Non-Convert |
   | F14               | IME Convert     |

4. 保存して PowerToys を常駐起動のままにする

> 環境によっては `IME On` / `IME Off` の項目が無いので、ここでは **無変換 (`IME Non-Convert`) / 変換 (`IME Convert`)** に再マップする。Microsoft IME 側で IME のオン・オフに紐付ける（次のステップ）。

### セットアップ 2：Microsoft IME のキー割り当て

1. `Win + I` → 時刻と言語 → 言語と地域
2. 「日本語」 → 言語のオプション
3. **Microsoft IME** → キーボードオプション → **キーとタッチのカスタマイズ**
4. 「**キーの割り当て**」のスイッチを **オン**
5. 以下を割り当てる：
   - **無変換キー** → **IME-オフ**
   - **変換キー** → **IME-オン**

> 「キーの割り当て」が機能しない・項目が出ない場合は、同じ画面の **「以前のバージョンの Microsoft IME を使う」** をオンにしてから再度設定する。

### 反映

設定後、**Windows をサインアウト → サインイン**（または再起動）が必要。設定だけ保存しても、起動済みのプロセスには適用されないことがある。

### 補足

- F+G コンボの `Ctrl+Space` トグルは PowerToys / IME 設定なしで OS のデフォルト IME 切替が動く
