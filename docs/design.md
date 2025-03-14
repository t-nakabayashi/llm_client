# ollama簡易クライアント設計書

## 要件定義書

### プロジェクトの目的
ollamaサーバーを駆動し、大規模言語モデル（LLM）のチャットを行うWebアプリケーションを開発する。ユーザーがブラウザ上でollamaの大規模言語モデルとチャットできるインターフェースを提供し、各種モデルを選択・設定できるようにする。

### 機能要件

#### チャット機能
- ブラウザにチャットウィンドウを表示
- ユーザーの入力をオウム返しする基本機能の実装
- シンプルなUIでのチャットインターフェース
- ストリーミングレスポンスのリアルタイム表示
- コードブロックの自動フォーマットとコピー機能

#### モデル連携機能
- ollamaのモデル選択機能
- 起動中のモデル一覧表示
- モデル終了機能
- GPU使用率のリアルタイム表示

#### 設定機能
- ollamaモデルのパラメータ設定（温度、top_p、top_k、コンテキスト長、繰り返しペナルティ）
- 設定の保存と適用

#### UI機能
- レスポンシブデザイン
- サイドバーの折りたたみ機能
- ダークモード対応（ブラウザの設定に準拠）

### 非機能要件
- 使いやすいUI/UX
- レスポンス性能の最適化
- エラーハンドリングの実装
- セキュリティ対策

### 制約条件
- Pythonを使用したバックエンド実装
- Webアプリケーションとしての実装
- ollamaサーバーとの連携

## 設計書

### 概略設計

#### システム構成
- **バックエンド**：Flask（Pythonウェブフレームワーク）
- **フロントエンド**：HTML, CSS, JavaScript
- **ollamaとの連携**：ollamaのPython API（ollama-python）

#### 技術スタック
- **言語**：Python 3.9+
- **Webフレームワーク**：Flask
- **フロントエンド**：HTML, CSS, JavaScript（必要に応じてBootstrap）
- **通信**：WebSocket（Flask-SocketIO）
- **ollamaクライアント**：ollama-python

### 機能設計

#### チャットインターフェース
- チャット履歴の表示
- メッセージ入力フォーム
- 送信ボタン
- チャット履歴のスクロール機能

#### モデル選択機能
- 利用可能なollamaモデルのリスト表示
- モデル選択ドロップダウン
- モデル情報の表示

#### パラメータ設定機能
- コンテキスト長の設定スライダー
- 温度（temperature）設定
- トップP（top_p）設定
- その他のパラメータ設定オプション

### クラス構成

#### `app.py`（メインアプリケーション）
- Flaskアプリケーションの初期化と設定
- ルーティング設定
- WebSocketイベントハンドラ
- モデル選択・管理API
- GPU情報取得API

#### `ollama_client.py`
- `OllamaClient`クラス：ollamaサーバーとの通信を担当
  - モデル一覧取得
  - 起動中モデル一覧取得
  - モデル終了機能
  - GPU情報取得
  - ストリーミングチャット実行
  - パラメータ設定

#### `chat_session.py`
- `ChatSession`クラス：チャットセッションの管理
  - メッセージ履歴の保持
  - コンテキスト管理
  - セッション設定

#### `static/js/chat.js`
- フロントエンドのチャット機能実装
- WebSocket通信
- UIの動的更新
- モデル管理機能
- サイドバー折りたたみ機能
- コードブロックのフォーマットとコピー機能

#### `templates/index.html`
- メインページのHTMLテンプレート
- チャットインターフェース
- モデル選択画面
- 設定画面
- モデル管理画面

### データフロー
1. ユーザーがブラウザでアプリケーションにアクセス
2. フロントエンドがバックエンドからモデル一覧と起動中モデル情報を取得
3. ユーザーがモデルを選択し、チャットを開始
4. ユーザーの入力がWebSocketを通じてバックエンドに送信
5. バックエンドがollamaサーバーにストリーミングリクエストを送信
6. ollamaサーバーからのチャンクレスポンスをリアルタイムでユーザーに返す
7. サイドバーでGPU使用率と起動中モデル情報を定期的に更新

### 実装済み機能

#### チャット機能
1. Flaskアプリケーションの基本構造
2. WebSocketによるリアルタイム通信
3. ストリーミングレスポンスの表示
4. コードブロックの自動フォーマットとコピー機能

#### モデル連携機能
1. ollamaクライアントの実装
2. モデル一覧取得と選択機能
3. 起動中モデル一覧表示と終了機能
4. GPU使用率のリアルタイム表示

#### 設定機能
1. パラメータ設定UI
2. 各種パラメータ設定機能
3. 設定の保存と適用

#### UI機能
1. レスポンシブデザイン
2. サイドバーの折りたたみ機能
3. ダークモード対応