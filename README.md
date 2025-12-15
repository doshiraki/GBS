# 🌌 GBS (GAS Boot System) - The Virtual OS Framework

> **"GAS開発を、最高のエンターテインメントへ。"**

GBSは、Google Apps Script (GAS) の制約を打破し、**「堅牢なアーキテクチャ」「爆速のパフォーマンス」「至高の開発者体験」** を提供するために設計された仮想OSフレームワークです。

従来の「モノリシックで壊れやすいGAS」を卒業し、モダンでスケーラブルなアプリケーション開発を実現します。

---

## 💎 特徴一覧 (Features & Examples)

### 1. 🏗️ 物理コピー戦略による「不変のインフラ」

コードを修正して保存した瞬間、ユーザー環境が壊れる恐怖とはお別れです。 GBSは**「稼働中のアプリ(v1)には指一本触れず、複製した別プロジェクト(v2)で開発する」**という運用を強制します 。

- **特徴:** 本番環境と開発環境が物理的に別のGASプロジェクトとして存在。
- **メリット:** デプロイミスによる本番停止リスクが物理的にゼロになります。 
- **例示:**    
    - 🛑 **従来のGAS:** `Code.gs` を修正 → 保存 → 本番環境に即反映（バグも一緒に公開😱）
    - ✅ **GBS:** `App_v1` (稼働中) をコピーして `App_v2` を作成 → 開発完了後、ルーター(`PartitionTable`)の向き先を `v1` から `v2` に切り替え 。

### 2. ⚡ 圧縮転送とインテリジェント・キャッシュ

「GASのWebアプリは遅い」という常識を覆します。 ソースコードはサーバー側で **Gzip圧縮 & Base64エンコード** され、クライアント側で展開されます 。

- **特徴:** 通信量を劇的に削減し、モバイル回線でも高速に動作。    
- **キャッシュ:** 一度読み込んだリソースは `localStorage` に保存され、2回目以降は爆速で起動します 。
- **Cache Buster:** バージョン番号 (`v1.0.1` -> `v1.0.2`) を変更するだけで、全ユーザーの古いキャッシュを自動的に破棄・更新します 。
### 3. 🪄 RPC Proxyによる「Async/Await」完全対応

古めかしいコールバック地獄 (`withSuccessHandler`) はもう不要です。

`google.script.run` をハイジャックし、現代的な `Promise` 構文を提供します 。

- **特徴:** サーバーサイドの関数を、あたかも同期処理のように記述可能。    
- **例示:**
    
    ```js
    // ❌ 従来のGAS (ネスト地獄)
    google.script.run.withSuccessHandler(data => {
      console.log(data);
      google.script.run.withSuccessHandler(result => {
        updateUI(result);
      }).processData(data);
    }).getData();
    
    // ✅ GBS (RPC Proxy)
    const data = await google.script.run.sync().getData();
    const result = await google.script.run.sync().processData(data);
    updateUI(result);
    ```
    

### 4. 🧭 Partition Table  によるルーティング

URLパラメータひとつで、過去のバージョンや検証環境へ自在にタイムスリップできます。

- **特徴:** `PartitionTable` ライブラリが、URLパラメータ (`?app=xxx`) と実際のGASプロジェクトIDを紐付けます 。
- **例示:**
    
    - `?app=PRD`: 最新の安定版 (`v2.0`) が起動 。
    - `?app=STG`: 開発中の次期バージョン (`v2.1-beta`) が起動。同じURLでテスト可能 。
    - `?app=v1`: 何かあった時のための旧バージョン (`v1.0`) も即座に呼び出し可能。

### 6. 🛡️ 仮想OSレイヤー (Stage 1.5 AppCore)

アプリケーション(`Stage 2`)は、面倒なインフラ処理を意識する必要がありません。

`AppCore` がファイルシステムの探索、テンプレートの合成、依存関係の注入を一手に引き受けます 。

- **特徴:** アプリ開発者は `Kernel` クラスと `render()` メソッドを書くだけ。
- **Chain of Responsibility:** システム領域のリソースを、ユーザー領域(アプリ)で上書き(オーバーライド)可能な柔軟な探索パスを持っています 。 

### 7. 🔌 ローカルブート機能 (Hybrid Boot)

巨大なシステムの一部でありながら、単体での開発・テストが可能です。

- **特徴:** `PartitionTable` に接続されていないときは、内蔵の `BIOS.js` が自動的に「ローカルモード」で起動します 。
- **メリット:** ルーターの設定をいじることなく、個々のアプリ開発(`Stage 2`)に集中し、「デプロイをテスト」ですぐに動作確認ができます。
    

---

## 🚀 Quick Start

### 1. プロジェクト構成

GBS準拠のアプリケーション(`Stage 2`)は、以下の5ファイルを基本セットとします 。

1. **`[AppName].js`**: Kernel Class (アプリの構成定義)    
2. **`[MainTemplateName].html`**: View (見た目のみ、ロジックなし)
3. **`[ServerLogic].js`**: Server Logic (サーバーサイド関数) 
4. **`BIOS.js`**: Local Loader (開発用ブートローダー)
5. **`export.js`**: Connector (外部公開定義)

### 2. Kernel Class のテンプレート

### [ 3. Integration Patterns (Code Recipes)](https://github.com/doshiraki/GBS_stage1.5/blob/main/README.md#3-integration-patterns-code-recipes)を参照

---

> **Enjoy your coding!** "コードは、ロジックであると同時にアートである。" - _GBS Architect_