# CatalogEasy

## 概要
<p>
商品の画像付きのカタログを作りたいが難しい操作をしたくない！という方へ向けたカタログ作成アプリです。

商品データと商品画像を登録することで簡単にカタログ作成ができます。
</p>

## オリジナルプロダクトの URL
https://app.catalog-easy.com

## TOPページ
<img src="./documents/design/top.png" width="70%"></img>

## 使用技術
### フロントエンド
  - TypeScript
  - Next.js(Page Router)
    - 主要ライブラリ ([選定理由](#選定理由)で解説)
      - Yamada UI、React Hook Form、React-Draggable、Axios、Prettier、ESLint、Lefthook
### バックエンド
  - Ruby on Rails
    - 主要gem ([選定理由](#選定理由)で解説)
      - devise_token_auth、rubocop、bycrypt、rack-cors
  - Nginx
### インフラ
  - AWS ([選定理由](#選定理由)で解説)
    - ECS Fargate、ECR、Amplify、RDS、ALB、Route53、ACM、S3
  - Mysql
  - Docker、Docker Compose 

## 選定理由
### 主要ライブラリ(フロントエンド)
  - Yamada UI
    - シンプルなUIを実現できるため
  - React Hook Form
    - 動的なフォームのバリデーションとフォームの状態管理を簡単に実装できるため
  - React-Draggable
    - ドラッグ&ドロップ機能を簡単に実装できるため
  - Axios
    - リクエスト前の前処理が簡単にできるため
  - Prettier、ESLint
    - コード品質担保のため
  - lefthook
    - コミット前に自動で静的解析ツールを走らせるため
  
### 主要gem(バックエンド)
  - devise_token_auth
    - 小規模アプリで、ユーザー認証が容易であるトークン認証を採用。
  - bycrypt
    - クライアント側でURLを暗号化させるため
  - rack-cors
    - APIの呼び出し時のCORS対応のため
  - rubocop
    - コード品質担保のため
 
### 主要AWSサービス(インフラ)
- ECS Fargate
  - Rails APIのデプロイのために使用。Docker開発におけるデプロイ負担の軽減のために選択。
- ECR
  - Rails API、Nginxコンテナイメージ保存のために使用。
- Amplify
  - Next.jsをデプロイするために使用。デプロイの容易さや、Vercelに比べ、AWSとのサービスとの連携が簡単にできるため選択。
- S3
  - 画像アップロードのために使用。VPCのエンドポイントからアクセスできるように設定。
- RDS(Mysql)
  - Rails APIのデータ保存のために使用。(マルチAZ)必要に応じてスケーリングが容易なため選択。
- ALB
  - ECS Fargateへのトラフィック分散を行うために使用。
- Route53
  - Rails、Next.jsのアプリケーションの独自ドメイン設定のために使用。
- ACM
  - SSL化のために使用。

## 機能一覧
### ユーザー利用機能
  - ログイン&ユーザー登録機能
  - 商品情報CSV読み込み機能
  - 商品画像/情報登録機能
  - カタログ作成機能
  - カタログ一覧機能
  - カタログPDF出力機能
  
### ユーザー非利用機能
  - UI
    - 親切な使用方法の解説
    - 機能を盛り込みすぎないことによる使いやすさ

  - セキュリティ
    - Route53、ACMを使用した独自ドメイン、SSL化対応
    - トークンを利用した認証
    - プラウザ側でのURLの暗号化
    - XSS、CSRF対策

  - 開発
    - 静的解析ツールを用いたコード品質担保
    - GitHub Actionsによる自動デプロイ
    - Amplifyによる自動デプロイ
    - Dockerを用いたデプロイ効率化
    - UIコンポーネント(Yamada UI)を用いた開発の簡略化
    - Lefthookを用いたコミット前の静的解析ツール実行

## ER 図
```mermaid
erDiagram
    USERS {
        bigint id PK "Primary Key"
        string provider "Default: email"
        string uid "Default: ''"
        string encrypted_password "Default: ''"
        string reset_password_token
        datetime reset_password_sent_at
        boolean allow_password_change "Default: false"
        datetime remember_created_at
        string confirmation_token
        datetime confirmed_at
        datetime confirmation_sent_at
        string unconfirmed_email
        string name
        string nickname
        string image
        string email
        text tokens
        datetime created_at
        datetime updated_at
    }

    PRODUCTS {
        bigint id PK "Primary Key"
        string name
        string size
        bigint trade_price
        bigint retail_price
        string remark
        datetime created_at
        datetime updated_at
        bigint user_id FK
    }

    CATALOGS {
        bigint id PK "Primary Key"
        string name
        bigint user_id FK
        datetime created_at
        datetime updated_at
    }

    PRODUCT_IMAGES {
        bigint id PK "Primary Key"
        datetime created_at
        datetime updated_at
        bigint user_id FK
    }

    IMAGE_ASSIGNMENTS {
        bigint id PK "Primary Key"
        bigint product_id FK
        bigint product_image_id FK
        string image_key
        datetime created_at
        datetime updated_at
    }

    CATALOG_SECTIONS {
        bigint id PK "Primary Key"
        bigint template_id FK
        bigint catalog_id FK
        bigint product_id FK
        bigint page_number
        bigint page_place_number
        datetime created_at
        datetime updated_at
    }

    TEMPLATES {
        bigint id PK "Primary Key"
        string content
        datetime created_at
        datetime updated_at
    }

    USERS ||--o{ PRODUCTS : "has many"
    USERS ||--o{ CATALOGS : "has many"
    USERS ||--o{ PRODUCT_IMAGES : "has many"
    CATALOGS ||--o{ CATALOG_SECTIONS : "has many"
    TEMPLATES ||--o{ CATALOG_SECTIONS : "has many"
    PRODUCTS ||--o{ CATALOG_SECTIONS : "has many"
    PRODUCTS ||--o{ IMAGE_ASSIGNMENTS : "has many"
    PRODUCT_IMAGES ||--o{ IMAGE_ASSIGNMENTS : "has many"
```

## インフラ構成図
<img src="./documents/design/arch.svg" width="70%"></img>

## 今後アップデートしたい内容
  - fetch使用部分の全面サーバーコンポーネント化
    - セキュリティ向上、fetch時のパフォーマンス向上のため
  - カタログ編集機能
  - テンプレート機能
  - 過去登録した商品情報からカタログ作成機能

