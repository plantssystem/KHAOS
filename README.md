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

44.1 kHz ステレオ。音のエンジンはハードウェア非依存の C++ で、Pico 版と Linux 版で同じファイルです。

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

## クレジット

created by osamu/plantssystem
