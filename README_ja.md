# TouchDesigner Watchdog

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TD Version](https://img.shields.io/badge/TouchDesigner-2023.12120-blue.svg)](https://derivative.ca/)

[English](README.md) | 日本語

TouchDesignerプロジェクトの監視(ウォッチドッグ)システムです。監視対象のTouchDesignerアプリケーションからのハートビート信号が途絶えた場合、自動的にプロセスを終了させ、再起動を行います。

<img width="563" alt="image" src="https://github.com/user-attachments/assets/47a6290d-6911-41e4-bb40-535a8fe0858f" />

## 目次
- [特徴](#特徴)
- [必要要件](#必要要件)
- [ファイル構成](#ファイル構成)
- [セットアップ方法](#セットアップ方法)
- [使用方法](#使用方法)
- [動作の仕組み](#動作の仕組み)
- [サンプルプログラム](#サンプルプログラム)
- [トラブルシューティング](#トラブルシューティング)
- [注意事項](#注意事項)
- [バージョン履歴](#バージョン履歴)
- [ライセンス](#ライセンス)

## 特徴
- 💫 TouchDesignerプロジェクトの自動監視
- 🔄 クラッシュ時の自動再起動
- 📡 UDPベースの軽量なハートビート通信
- 🔌 簡単な統合（drag & drop）

## 必要要件

- TouchDesigner 2023.12120で動作確認済み（他のバージョンでも動作する可能性があります）
- macOS(Windowsには対応していません)

## ファイル構成

- `watchdog.toe` - メインのウォッチドッグシステム。監視対象アプリケーションからのハートビート信号を監視し、信号が途絶えた場合にプロセスの終了・再起動を行います
- `watchdog-heartbeat-sender.tox` - 監視対象アプリケーションに組み込むためのハートビート送信コンポーネント
- `watchdog-target.toe` - ハートビート送信コンポーネントを組み込んだサンプルアプリケーション

## セットアップ方法

### 1. 監視対象アプリケーションの設定

1. 監視対象のTouchDesignerプロジェクトを開きます
2. `watchdog-heartbeat-sender.tox`をTouchDesignerのプロジェクトウィンドウに直接ドラッグ&ドロップします
   - コンポーネントが自動的にプロジェクトに追加されます
3. 追加されたコンポーネントを選択し、以下の設定を行います:
   - IPアドレス: ウォッチドッグシステムが動作するPCのIPアドレス
   - ポート番号: 使用するUDPポート番号
4. プロジェクトを保存します

### 2. ウォッチドッグの設定

1. `watchdog.toe`を開きます
2. Target Applicationセクションで以下の設定を行います:
   - Absolute Path: 監視対象アプリケーションの実行ファイルパスを設定
     - デフォルトでは`/path/to/watchdog-target.toe`となっているため、実際の監視対象ファイルのパスに変更してください
   - ポート番号: ハートビート送信側と同じポート番号を設定

## 使用方法

1. 監視対象のTouchDesignerプロジェクトを起動します
2. `watchdog.toe`を起動します
3. ウォッチドッグシステムが自動的にハートビート信号の監視を開始します
4. 監視対象プロジェクトが応答を停止した場合:
   - プロセスを強制終了します
   - 自動的に再起動を行います

## 動作の仕組み

1. 監視対象プロジェクトに組み込まれた`watchdog-heartbeat-sender.tox`が定期的にUDPパケットを送信します
2. `watchdog.toe`がハートビート信号を監視します
3. 30秒以上ハートビート信号を受信できない場合、異常と判断します
4. 異常検知時:
   - 監視対象プロセスを強制終了します
   - 自動的に再起動処理を実行します

## サンプルプログラム

`watchdog-target.toe`は、ハートビート送信コンポーネントを実際に組み込んだサンプルアプリケーションです。このサンプルを参考に、独自のプロジェクトへの実装を行うことができます。

## トラブルシューティング

### ハートビート信号が検出されない場合
- ファイアウォールの設定を確認してください
- IPアドレスとポート番号が正しく設定されているか確認してください
- ネットワーク接続を確認してください

### プロセスの再起動が失敗する場合
- 絶対パスが正しく設定されているか確認してください
- ファイルの権限設定を確認してください

## 注意事項

- ネットワーク設定(ファイアウォール等)でUDP通信が許可されていることを確認してください
- プロセスパスは絶対パスで指定する必要があります
- 本番環境で使用する前に、十分なテストを行ってください
- 現在このシステムはmacOSのみに対応しています。Windows版は現在対応していません
- 本システムの使用は自己責任で行ってください。本システムの使用によって生じたいかなる損害や損失について、作者は一切の責任を負いません

## バージョン履歴

- v1.0.0
  - 初回リリース
  - 基本的なウォッチドッグ機能
  - macOSサポート

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。詳細は[LICENSE](LICENSE)ファイルを参照してください。
