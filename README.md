# 家計簿アプリ

スマートフォン対応の家計簿ウェブアプリです。

## 機能
- 支出の入力・カテゴリ管理
- 毎月固定費の自動入力（📌 固定機能）
- グラフで過去6ヶ月の推移を確認
- 領収書・手書き家計簿をカメラで撮影してAI自動入力（スキャン機能）
- Firebase連携で夫婦・家族とデータ共有

## 使い方

### ローカルモード（すぐ使える）
`index.html` をブラウザで開くか、GitHub Pages の URL にアクセスするだけで使えます。  
データはブラウザのローカルストレージに保存されます。

### スキャン機能を使う
1. [Anthropic Console](https://console.anthropic.com) で APIキー（`sk-ant-...`）を発行
2. アプリの「共有」タブ → 「APIキーを設定」に貼り付け
3. カメラアイコンで家計簿を撮影 → AI が自動読み取り

### 夫婦で共有する（Firebase 設定）
1. [Firebase Console](https://console.firebase.google.com) でプロジェクトを作成
2. Authentication（メール/パスワード）と Firestore を有効化
3. `index.html` の `window.FIREBASE_CONFIG` の各値を書き換える
4. Firestore のセキュリティルールを設定する（下記参照）

#### Firestore セキュリティルール
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
    match /households/{hid} {
      allow read, write: if request.auth != null
        && request.auth.uid in resource.data.members;
      allow create: if request.auth != null;
    }
  }
}
```

## GitHub Pages へのデプロイ

1. リポジトリの **Settings → Pages** を開く
2. **Source** を `GitHub Actions` に変更して保存
3. `main` ブランチにプッシュすると自動デプロイされます

デプロイ後の URL: `https://<your-username>.github.io/<repository-name>/`
