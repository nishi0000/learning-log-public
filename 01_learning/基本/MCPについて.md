### 0. スレッド概要

- 扱ったテーマ：
    
    - MCP / LLM の定義
        
    - LLM と検索（Google）の違い
        
    - MCP は自作できるか（何を作るか・最低限の設計観点）
        
    - MCPサーバが公開できる機能（Tools / Resources / Prompts）
        
    - MCPの基本構造（クライアント/サーバ、ツール実行の流れ）
        
    - MCP運用での最低限のセキュリティ観点（最小権限・入力検証など）
        

### 1. 扱ったトピック一覧（時系列）

-  トピック：MCPとは
    
    - 何を扱った：
        
        - MCP（Model Context Protocol / えむしーぴー）：LLMが外部の道具やデータにアクセスして作業するための「共通ルール（プロトコル）」
            
        - できること例：ローカルファイル/フォルダ、DB検索、社内ドキュメント参照、独自ツール（社内API等）呼び出し、結果を使った回答生成
            
        - 構造：MCPクライアント（LLM側）⇔ MCPサーバ（ツール提供側）
            
    - 結論：
        
        - MCPは「LLM ↔ ツール/データ」の接続方法を標準化したもの、という理解で整理した
            
    - 覚えるもの：
        
        - MCP → Model Context Protocol → えむしーぴー → LLMがツールを使うための標準接続
            
-  トピック：LLMとは
    
    - 何を扱った：
        
        - LLM（Large Language Model / えるえるえむ）：大量の文章を学習し、文章を理解・生成するモデル
            
        - 得意：要約、整理、比較、文章作成、コード生成など
            
        - 注意：日付・数字・固有名詞・最新状況は誤りが混ざる可能性があるため確認が必要、という扱い
            
        - 例として言及：OpenAI の ChatGPT、Anthropic の Claude、Google の Gemini
            
    - 結論：
        
        - LLMは「回答を生成する」側。事実確認が必要な領域がある
            
    - 覚えるもの：
        
        - LLM → Large Language Model → えるえるえむ → 文章理解/生成が中心、最新・数値は要確認
            
-  トピック：LLMと普通の検索（Google）の違い
    
    - 何を扱った：
        
        - 検索：既存情報を見つけてリンクで案内（一次情報・根拠追跡に強い）
            
        - LLM：学習知識＋入力から文章を組み立てて回答（要約・構造化・下書きに強い）
            
        - 使い分け：正しさ/出典が必要→検索、まとめて形にしたい→LLM、が基本
            
        - 強い組み合わせ：検索で一次情報収集 → LLMで要約/手順化/比較表化
            
    - 結論：
        
        - 「探す（検索）」と「まとめて答える（LLM）」で役割が違う、という整理にした
            
    - 覚えるもの：
        
        - 検索＝一次情報・最新・根拠
            
        - LLM＝要約・整理・草案
            
        - 併用＝検索→LLM整形
            
-  トピック：MCPは作れるか（自作の対象と最小の流れ）
    
    - 何を扱った：
        
        - 「作れる」＝MCPサーバ（ツール提供側）を自作する、という位置づけ
            
        - MCPサーバが公開できるもの：Tools / Resources / Prompts
            
            - Tools：関数呼び出し（例：DB検索、ファイル検索、社内API呼び出し）
                
            - Resources：読み取りデータ（例：README、設計書、ログ）
                
            - Prompts：定型プロンプト（例：レビュー観点、要約テンプレ）
                
        - 作り方の最小手順（スレッド内で言及した粒度）：
            
            - どのツールを出すか決める（引数/戻り値をスキーマ化）
                
            - SDKでサーバ実装（TypeScriptが定番、という言及）
                
            - ホスト（例：Claude Desktop など）から接続設定して呼ぶ（stdio接続の言及）
                
        - スレッド内でURLが提示された（参照として会話内に登場）：
            
            - [https://modelcontextprotocol.io/docs/develop/build-server](https://modelcontextprotocol.io/docs/develop/build-server)
                
            - [https://modelcontextprotocol.io/specification/2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
                
            - [https://www.npmjs.com/package/%40modelcontextprotocol/sdk](https://www.npmjs.com/package/%40modelcontextprotocol/sdk)
                
            - [https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
                
            - [https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)
                
    - 結論：
        
        - MCPは「LLMが使う道具箱サーバ」を作る形で自作できる、という方向で合意した
            
    - 覚えるもの：
        
        - MCP自作＝MCPサーバ実装
            
        - 公開単位＝Tools / Resources / Prompts
            
        - 実装の要点＝ツール定義（スキーマ）→実装→ホストから接続
            
-  トピック：MCPのセキュリティ観点（最低限）
    
    - 何を扱った：
        
        - MCPサーバは“実行権限を持つ道具”になりやすい、という前提
            
        - 最初から守る観点として列挙：
            
            - 最小権限（読み取り専用から開始、書き込み/削除は分離）
                
            - 入力バリデーション（パス、コマンド引数、SQL条件を厳格に）
                
            - 危険操作のガード（許可リスト方式、作業ディレクトリ固定、タイムアウト）
                
            - 秘密情報を出力に混ぜない（ログ含む）
                
            - 失敗時は閉じる（fail closed）
                
    - 結論：
        
        - MCPサーバは「できること」を増やすほど危険操作が入りやすいので、最小権限＋検証＋ガードを先に置く、という整理になった
            
    - 覚えるもの：
        
        - 最小権限 / 入力検証 / 許可リスト / 秘密情報を出さない / fail closed
            
