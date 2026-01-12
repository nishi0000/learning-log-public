
# 実装ログ：GitHub公開に向けたリポジトリ整理（Vue + Lambda + Obsidian）

## 0. このログの対象

このスレッドで実際にやったのは、アプリのコードと技術ログ（Obsidian）を **GitHubで管理・公開できる形に整理**すること。  
「GitHubで管理したい」→「ファイル構成どうする」→「git init/commit/push」→「公開したら過去ログ見える？」→「ObsidianのPublic Vault分離」まで。

---

## 1. やろうとしていたこと（最初の目的）

- Vue（Vite）のフロントと、C#/.NETのLambdaバックエンドを **GitHubで管理できる形にしたい**
    
- Publicにする予定なので、**入れたくないもの（個人ログや自動生成物）を混ぜない**ようにしたい
    
- 技術ログは Obsidian で管理していて、最初はPrivateでいろいろ入れていたが、Public化するため整理したい
    

---

## 2. 最初に決めた構成（認識の確定）

### 2.1 「みんながよくやる構成」を採用

- 1リポジトリ（モノレポ）で `frontend/` と `backend/` を分けるのが分かりやすい、という方針にした。
    

最終的に目指した形：

```
MYAPP/
  frontend/   # Vue(Vite)
  backend/    # Lambda(.NET)
  infra/      # いったん空でもOK
  myapp.sln
  .gitignore
  README.md（後で）
```

---

## 3. 実際に起きた詰まり：フォルダ移動で EPERM

### 3.1 起きたこと

`TodoApi` を `backend/TodoApi` に移動（rename）しようとして、VSCode側で **EPERM（permission error）** が出た。

- 表示内容：`rename '...TodoApi' -> '...backend\TodoApi'` が失敗
    
- 体感としては「移動したいだけなのに、なぜか拒否される」
    

### 3.2 修正して通ったこと

- 実行中のプロセスを止める（dotnet run/watch、デバッグなど）
    
- ターミナルのカレントディレクトリをTodoApi配下から外す
    
- 最終的には移動が完了して `backend/TodoApi` になった
    

---

## 4. `.gitignore` で詰まった：何を書けばいい？何が消える？

### 4.1 目的

Publicにするので、最低限これらはGitに入れない：

- `node_modules/`（依存が巨大）
    
- `dist/`（ビルド成果物）
    
- `.NET` の `bin/` `obj/`
    
- ログやローカル設定
    
### 4.2 実際に整えた `.gitignore`（ルート）

最終的にこういう形に整えた（例）：

```gitignore
# Logs
logs/
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log*

node_modules/
dist/
backend/**/bin/
backend/**/obj/

# Editor
.vscode/*
!.vscode/extensions.json
.idea
*.suo
*.ntvs*
*.njsproj
*.sw?

# etc
coverage/
.eslintcache
```

### 4.3 途中で直したポイント

- 以前 `*.sln` を入れてしまいそうになった  
    → `myapp.sln` をGitに入れたいので、**`*.sln` は入れない**判断にした  
    （「何を共有したいか」を意識するポイントだった）
    

---

## 5. Git操作で「これでいいの？」になった

### 5.1 起きたこと

- VSCodeで `git init` を押したら、ソース管理画面が出てきて「コミットでいいのか？」になった
    

### 5.2 何が起きていたか（状態）

- `git init` をしただけの状態＝「管理開始しただけ」
    
- `git status` を見たら、Untracked が以下だけで、余計なものが入ってないのを確認できた：
    
    - `.gitignore`
        
    - `backend/`
        
    - `frontend/`
        
    - `myapp.sln`
        

→ ここで「`.gitignore` は効いてる（node_modulesやdistが出てない）」と判断できた。

### 5.3 実際に使ったコマンド

```powershell
git add .
git commit -m "Initial commit"
git branch -M main
```

---

## 6. GitHubの「草」と「リポジトリ削除」で不安になった

### 6.1 不安

- GitHubでリポジトリを削除したら、草（contributions）も消えるのか？
    
- 消したリポジトリは戻せるのか？
    

### 6.2 実際に起きたこと

- GitHub画面上でリポジトリを削除 → 直後に「復元が出てこない」状態になって焦った
    
- 結果として、少しして **リストアできた**（復元が反映されるまでタイムラグがある挙動だった）
    

### 6.3 この時の学び（ここがズレポイント）

- 「削除したら即復元できる」と思ってたが、UI側の反映にタイムラグがある
    
- Public運用をするなら、削除より **Archive** のほうが安全な場面がある（後述）
    

---

## 7. Archive（アーカイブ）とは何だったか

- リポジトリを削除せずに **読み取り専用で凍結**する機能
    
- 「残したいけど触りたくない」「公開のまま保管」みたいな用途
    

---

## 8. 一番重要だった認識：Publicにすると“過去”は見える

### 8.1 ここが核心の認識ズレ

- 「ファイルを消して `.gitignore` に書けば、Publicにしても過去は見えない」  
    → **これは違う**
    

### 8.2 修正して理解したこと

- GitHubで見えるのは **最新のファイルだけじゃなくて、コミット履歴（過去のスナップショット）**
    
- `.gitignore` は未来の追跡を止めるだけで、過去コミットの内容は消さない
    
- つまり、Privateの間にpushした内容があるなら、Public化で見える可能性がある
    

### 8.3 取った方針（安全策）

- 「今のPrivate運用のものはPrivateのまま温存」
    
- **公開したいものは新しいPublicリポジトリに分離**するのが一番安全
    

---

## 9. Obsidian運用の結論：Vaultを分けるのが安全

### 9.1 何が問題だったか

- 同じVaultの中に個人ログも技術ログも混ざっていると、Gitで巻き込み事故が起きやすい
    

### 9.2 取った方針

- Private Vault（個人ログ・雑多なもの）＝Privateのまま
    
- Public Vault（公開していい整理済みログ）＝新規で作ってPublicリポジトリにする
    

### 9.3 Public Vault側の `.gitignore` 例（実際に出てきたやつ）

Publicに出したくないフォルダを除外する方針：

```gitignore
/00_template/
/98_doc/
/99_meta/
/.obsidian/
```

### 9.4 ここで起きた小さい不安

- 「`.gitignore` までアップロードしちゃった」  
    → `.gitignore` はむしろ **リポジトリに入れるのが普通**（問題なし）
    

---

## 10. 今回の“詰まりポイント”まとめ（再利用用）

- **EPERM**：フォルダ移動は「実行中/ロック中」だと失敗する
    
- **.gitignoreの誤解**：書けば過去も消えると思ってたが、消えない（未来だけ）
    
- **Public化の誤解**：「mainだけ公開」ではなく、履歴も含めて見える前提で考える必要がある
    
- **ObsidianはVault分離が安全**：公開・非公開を混ぜないのが一番ラク
    

---

## 11. このログを次に活かすためのキーワード

- 「gitignoreは未来、履歴は過去」
    
- 「Publicは履歴が資産にもリスクにもなる」
    
- 「安全策は “別リポジトリ/別Vault” が最強」
    

---

必要なら、このログをあなたの「実装ログ用テンプレート」形式にそのまま流し込んだ版も作れる（テンプレ貼ってくれたら、それに合わせて整形する）。