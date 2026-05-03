# Common Library for TypeScript (Angular)

> **"Ensuring front-end consistency and type-safe communication."**  
> 本リポジトリは、Angular を採用する全フロントエンドプロジェクトに対して、共通の基盤機能と型定義を提供するライブラリである。

---

## 📌 目的
応用情報技術者試験（AP）の「ヒューマンインタフェース設計」および「ソフトウェア再利用」の原則に基づき、13のリポジトリでバラバラになりがちなUIの振る舞いや、API通信ロジック、認証処理を共通化する。これにより、UXの統一とフロントエンド開発の信頼性向上を実現する。

## 📦 提供機能

### 1. API Communication (`api/`)
- **`CommonHttpInterceptor`**: 全てのHTTPリクエストに自動で `X-Trace-Id` と JWT を付与し、共通形式のエラーハンドリングを行う。
- **`ApiModels`**: Java側の `ApiResponse<T>` と対になる、TypeScript用のインターフェース定義。

### 2. Auth & Guard (`auth/`)
- **`AuthService`**: トークンの保存（SessionStorage等）と有効期限の管理。
- **`RoleGuard`**: AP試験の「アクセス制御」に基づいた、権限によるルーティング制限。

### 3. Utility & Logging (`logging/`)
- **`LoggerService`**: 環境（dev/prod）に応じたログレベル制御と、実行時エラーのサーバーへの転送。

## 🛠️ セットアップ

### 1. ライブラリのインストール
```bash
npm install @example/common-library-ts
```

### 2. Angular AppModule へのインポート
```typescript
@NgModule({
  imports: [
    CommonLibModule.forRoot({
      apiUrl: environment.apiUrl,
      appId: 'service-a'
    })
  ]
})
export class AppModule { }
```
## 📐 実装規約
AP試験の「保守プロセス」に基づき、本ライブラリの利用者は以下のルールを守ること。

1. **型定義の尊重**: 
   APIレスポンスは any 型を禁止し、必ず本ライブラリで定義されたインターフェース（Generics）を使用すること。
2. **UIの一貫性**: 
   共通化されたHTTPインターセプターをバイパスせず、エラーメッセージの表示形式をシステム全体で統一すること。
3. **副作用の排除**: 
   ライブラリ内のサービスはステートレスを基本とし、特定の業務ドメインに依存する状態を持たせないこと。

---
© 2026 Standard Architecture Blueprint. Managed by System Architect.```
