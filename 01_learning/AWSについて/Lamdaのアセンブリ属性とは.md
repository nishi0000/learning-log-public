
# テーマ

AWS Lambda（C#）では、JSONは誰がどこで変換しているのか

## 目的

Lambda のコードを読んだときに、  
「JSONをどこで・誰が・いつパースしているのか」を混乱せずに理解するため。

## 本筋

- Lambda の C# コード内で JSON を自分でパースしているのか？
    
- アセンブリ属性で書いている設定は、何を決めているのか？
    

## 結論

Lambda では、**API Gateway から渡ってくる JSON 全体は、自分のコードが動く前に Lambda 側で C# のオブジェクトに変換されている**。  
その変換ルールを、アセンブリ属性で「このやり方を使ってね」と最初に指定している。  
だから `FunctionHandler` の引数として、最初から `APIGatewayProxyRequest` をそのまま受け取れている。

## 図解

```text
API Gateway
   ↓（JSON文字列）
Lambda実行環境
   ├─ アセンブリ属性を読む
   ├─ JSON → APIGatewayProxyRequest に変換
   ↓
FunctionHandler(request)
```

## 詳細

- API Gateway から来る時点では、データはただの JSON（文字列）
    
- Lambda は `FunctionHandler` を呼ぶ前に、
    
    - 「この Lambda はどの JSON 変換方法を使うか」を確認
        
    - JSON を `APIGatewayProxyRequest` に変換
        
- そのため、コード内では
    
    - `request.HttpMethod`
        
    - `request.Body`  
        などを最初から普通の C# オブジェクトとして扱える
        
- `request.Body` の中身を、さらに自分のクラスにしたい場合だけ、自分で JSON パースをする
    
