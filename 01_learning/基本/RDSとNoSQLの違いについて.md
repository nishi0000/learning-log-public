
# テーマ

RDSとNoSQLはAPI設計でどう役割分担するのか？

## 目的

AWSでAPIを作るときに、  
「RDSで作るべきか？NoSQL（DynamoDB）を使うべきか？」で迷わなくなるため。

## 本筋

- RDSとDynamoDBは何が違うのか
    
- API単位で、どちらを使うのかをどう判断するのか
    
- ツイート＋いいね機能を例に、実際のDB設計とAPIの流れを理解する
    

## 結論

RDSとDynamoDBはどちらかを選ぶものではなく、**APIの中で役割分担して同時に使うもの**。  
一覧・検索・人が見るデータはRDS、  
IDを渡して即更新される・連打される処理はDynamoDB。  
DB同士はJOINせず、**API（Lambda）がデータを合体する**。

## 図解

```text
Client
  ↓
API Gateway
  ↓
Lambda
  ├─ RDS
  │    └ ツイート本文・一覧・検索
  └─ DynamoDB
       └ いいね数（+1 / -1）

Lambdaで合体してレスポンス
```

## 詳細

- RDSは、あとから条件が増える・並び替えたい・一覧表示したいデータ向き
    
- DynamoDBは、主キー1つで即取得・即更新したいデータ向き
    
- ツイート機能の例
    
    - RDS
        
        - tweet_id
            
        - user_id
            
        - content
            
        - created_at
            
        - deleted_at
            
    - DynamoDB
        
        - PK: tweet_id
            
        - like_count
            
- ユーザーのつぶやき一覧APIでは
    
    1. RDSでツイート一覧を取得
        
    2. 取得したtweet_idを使ってDynamoDBからいいね数を取得
        
    3. Lambdaで合体して画面用データを作る
        
- いいね操作APIでは
    
    - RDSは触らない
        
    - DynamoDBでlike_countを原子的に+1 / -1する
        
- DynamoDBは「同じキーにアクセスが集中すると詰まる（ホットパーティション）」ため、  
    バズる前提の設計が必要
    
