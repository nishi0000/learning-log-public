### 0. スレッド概要

- 扱ったテーマ：IPsec（AH・ESP）と他セキュリティ技術（SSH / S/MIME / XML暗号）の識別
    

---

### 1. 扱ったトピック一覧（時系列）

-  トピック：FE過去問（平成31年春 午前 問43）
    
    - 何を扱った：  
        [https://www.fe-siken.com/kakomon/31_haru/q43.html](https://www.fe-siken.com/kakomon/31_haru/q43.html)  
        AH・ESPを用いるセキュリティ技術の識別問題
        
    - 結論：  
        AH と ESP を用いるのは **IPsec**
        
    - 覚えるもの：  
        AH・ESP → IPsec
        

---

-  トピック：IPsecの構成要素
    
    - 何を扱った：AH / ESP の役割
        
    - 結論：
        
        - AH：認証・改ざん検知
            
        - ESP：暗号化・認証
            
    - 覚えるもの：  
        AH = Authentication Header（認証・改ざん検知）  
        ESP = Encapsulating Security Payload（暗号化・認証）
        

---

-  トピック：IPsecの位置づけ
    
    - 何を扱った：どの層・何を守るか
        
    - 結論：  
        IPsec は IP通信（ネットワーク層）を保護する仕組み
        
    - 覚えるもの：  
        IPsec → ネットワーク層 / IPパケット単位で保護
        

---

-  トピック：他方式との違い（選択肢の切り分け）
    
    - 何を扱った：SSH / S/MIME / XML暗号との用途比較
        
    - 結論：
        
        - SSH：リモートログインの安全化
            
        - S/MIME：電子メールの安全化
            
        - XML暗号：XMLデータの暗号化
            
        - IPsec：IP通信の安全化
            
    - 覚えるもの：  
        SSH → リモートログイン  
        S/MIME → メール  
        XML暗号 → XML  
        IPsec → IP通信（AH・ESP）
        

---

-  トピック：試験での判別キーワード
    
    - 何を扱った：問題文からの判断方法
        
    - 結論：  
        AH / ESP / VPN / IPパケット などの記述があれば IPsec
        
    - 覚えるもの：  
        AH・ESP・VPN → IPsec
        

---

### 最小暗記セット（このスレッドの結論）

- AH・ESP → IPsec
    
- AH = Authentication Header（認証・改ざん検知）
    
- ESP = Encapsulating Security Payload（暗号化・認証）
    
- IPsec → ネットワーク層（IP通信）
    
- SSH → リモートログイン
    
- S/MIME → メール
    
- XML暗号 → XMLデータ