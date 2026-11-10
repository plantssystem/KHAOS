# KHAOS

**Noise / drone synth with chaos modulation** — Raspberry Pi Pico 2 and Linux (AppImage).

2つのオシレーターが互いにFMし合い、カオス写像で揺れ続けるノイズ/ドローン・シンセです。
Raspberry Pi Pico 2 で動く本体と、同じ音のエンジンをそのまま使った Linux デスクトップ版(AppImage)があります。

サイト: <https://plantssystem.github.io/KHAOS/>

## 特長

- **クロスFMオシレーター** — 2つのオシレーター(SAW / PULSE / TRI)が互いの出力で周波数を変調。RATIO で周波数比を変更
- **CHAOS CV** — ステップ状のランダム電圧をピッチ・フィルター・FM に配分。LFSR / LOGISTIC(ロジスティック写像)/ NOISE の3モード、RATE で速さを変更
- **フィードバック + 共振SVF** — ステート・バリアブル・ローパスの出力を入力とオシレーターへ戻す。RESO を上げるとほぼ自己発振
- **CRUSH** — サンプルレートとビット深度を同時に落とす(15 bit → 3 bit)
- **BURST / FREEZE** — ノイズを一発鳴らす / カオスCVとワイヤーフレームの回転を固定
- **画面** — スコープ、CHAOS CV の履歴、9本のパラメーターバー、波形に連動して回るワイヤーフレーム(SAW=四面体、PULSE=立方体、TRI=八面体)
- **起動スプラッシュ** — ロジスティック写像の分岐図が左から右へ描かれ、タイトルがRGBグリッチから現れる

44.1 kHz ステレオ。音のエンジン(`monos_dsp.h`)はハードウェア非依存の C++ で、Pico 版と Linux 版で同じファイルです。

### パラメーター

| | | | | | | | | |
|---|---|---|---|---|---|---|---|---|
| PITCH | RATIO | XFM | FEEDBACK | CUTOFF | RESO | CHAOS | RATE | CRUSH |

## Linux 版

[Releases](https://github.com/plantssystem/KHAOS/releases/latest) から `KHAOS-*-x86_64.AppImage` をダウンロードします。

```sh
chmod +x KHAOS-*.AppImage
./KHAOS-*.AppImage
```

音声は SDL2 経由(PulseAudio / PipeWire / ALSA)で出力します。

### 操作

| キー | 動作 |
|---|---|
| ← / → | パラメーター選択 |
| ↑ / ↓ | 値を調整(押し続けると加速) |
| A | FREEZE |
| B | BURST |
| X | CHAOS モード切替(LFSR / LOGISTIC / NOISE) |
| Y | 波形切替(SAW / PULSE / TRI) |
| Space / Enter | 全パラメーターをランダム(Pico の CTRL) |
| M | ミュート |
| I | 初期値に戻す |
| F / F11 | フルスクリーン |
| Esc / Q | 終了 |

マウス: バーをクリック/ドラッグで値を設定、ホイールで選択中の値を調整。

## Raspberry Pi Pico 2 版

`pico/KHAOS/` を Arduino IDE で開いて書き込みます。

**ハードウェア**

- Raspberry Pi Pico 2
- Waveshare Pico-Audio(DOUT = GP26, BCLK = GP27, LRCK = GP28)
- Waveshare Pico-LCD-1.3(240×240 ST7789、ボタン + ジョイスティック)

**Arduino IDE の設定**

- ボード: Raspberry Pi Pico 2(Earle Philhower の "Raspberry Pi Pico/RP2040/RP2350" コア)
- ツール → USB Stack: **Adafruit TinyUSB**(Serial + MIDI)
- ライブラリ: Adafruit GFX Library / Adafruit ST7735 and ST7789 Library / Adafruit TinyUSB Library / MIDI Library(FortySevenEffects)

core1 が音声、core0 が画面とボタンを担当します。画面の向きが違う場合は `KHAOS.ino` の `LCD_ROTATION` を変更してください。

### USB-MIDI(Pico 版のみ)

USB-MIDI デバイス名は `KHAOS`、チャンネル 1 です。

| 入力 | 動作 |
|---|---|
| ノート 60–62 | 波形(SAW / PULSE / TRI) |
| ノート 70 | FREEZE 切替 |
| ノート 71 | CHAOS モード切替 |
| ノート 72 | BURST |
| CC 7 | 音量(100 が標準) |
| CC 20 | CHAOS 量(64 が標準) |
| CC 21 | PITCH(64 が標準) |
| CC 22 | RATE(64 が標準) |
| CC 23 | 波形(0 = SAW, 1 = PULSE, 2 = TRI) |
| CC 30 / 31 / 32 | 全ランダム / 初期値 / ミュート |

出力: ノート 36–43 = CHAOS CV の位置(ベロシティ = 出力レベル)、ノート 60+波形 / 64+モード / 70(FREEZE)/ 72(BURST)は状態通知(変化時と1秒ごと)。

## 構成

```
pico/KHAOS/        Pico 2 版スケッチ(KHAOS.ino, monos_dsp.h, khaos_splash.h)
src/               Linux 版(monos_dsp.h / khaos_splash.h は pico/KHAOS と同一)
src/compat/        Adafruit_GFX 互換の最小シム(キャンバス + 5x7 フォント)
src/khaos_app.h    KHAOS.ino の UI を移した、プラットフォーム非依存の本体
src/main_sdl.cpp   SDL2(ウィンドウ・音声・キーボード/マウス)
test/headless.cpp  SDL なしで画面と音を書き出す確認用
packaging/         .desktop とアイコン
index.html         GitHub Pages のサイト
```

## クレジット

created by osamu/plantssystem
