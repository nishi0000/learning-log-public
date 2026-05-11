
# AWS デプロイ用アプリ作成チャート

## 最終目的
- ポートフォリオとして公開できる Web アプリを作る
- 普段使わない AWS サービスを使い、構成理解を示せるようにする

---

## 全体構成（使用技術）

```text
[ブラウザ]
   │
   ├─ フロントエンド
   │    └ Vue（Vite）
   │         └ S3（静的Webホスティング）
   │
   └─ バックエンド API
        └ API Gateway（HTTP API）
             └ Lambda（C#）
                  └ （将来：DBなど）
````

---

## 作業チャート（順序のみ）

### A. フロントエンド

- A1. Vue（Vite）プロジェクト作成
    
- A2. `npm run build` で `dist` 生成
    
- A3. S3 バケット作成
    
- A4. S3 静的Webホスティング設定
    
- A5. `dist` を S3 に配置
    
- A6. ブラウザから表示確認
    

---

### B. AWS 基本設定

- B1. AWS アカウント作成
    
- B2. root アカウントで MFA 有効化
    
- B3. IAM ユーザー作成（CLI 用）
    
- B4. IAM 権限付与
    
- B5. Access Key / Secret Key 発行
    
- B6. `aws configure`
    
- B7. CLI 動作確認
    

---

### C. バックエンド（Lambda）

- C1. C# Lambda プロジェクト作成
    
- C2. `Function.cs`（エントリーポイント）確認
    
- C3. ローカルビルド確認
    
- C4. Lambda 関数としてデプロイ
    
- C5. IAM Role / Policy 設定
    

---

### D. API Gateway

- D1. HTTP API 作成
    
- D2. Lambda と統合
    
- D3. ルート作成（GET / POST）
    
- D4. ステージ作成・デプロイ
    
- D5. Invoke URL 発行
    

---

### E. 動作確認・調整

- E1. API Gateway → Lambda 到達確認
    
- E2. Lambda のレスポンス確認
    
- E3. エラー時のログ確認（CloudWatch）
    
- E4. レスポンス形式調整（JSON）
    

---

## ChatGPT に渡すときの使い方

```
このチャートで進めています。
いま C2 まで終わっています。
次の C3 から教えてください。
```

```
このチャートの D フェーズについて、
構成と考え方だけ説明してください。
```

---

## このノートのルール

- 進捗や感想は書かない
    
- 事実（構成・順序）のみを書く
    
- 変更があったらチャートを書き換える
    
