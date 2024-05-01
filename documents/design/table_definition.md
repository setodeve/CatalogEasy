### User テーブル
| フィールド名 | データ型 | キー | 説明         | null許容 |
|----------|-------|----|------------|-------|
| id       | bigint | PK | ユーザーID     | ×     |
| name     | string |    | ユーザー名     | ×     |
| email    | string |    | メールアドレス  | ×     |
| hashed_password    | string |    | パスワード  | ×     |

### Catalog テーブル
| フィールド名 | データ型 | キー | 説明       | null許容 |
|----------|-------|----|----------|-------|
| id       | bigint | PK | カタログID   | ×     |
| name     | string |    | カタログ名   | ×     |
| user_id  | references | FK |ユーザーID   | ×     |

### Product テーブル
| フィールド名 | データ型 | キー | 説明     | null許容 |
|----------|-------|----|--------|-------|
| id       | bigint | PK | 製品ID   | ×     |
| image_id  | references | FK | 画像ID   | ○     |
| name     | string |    | 製品名   | ○     |
| size     | string |    | サイズ   | ○     |
| trade_price    | bigint |    | 卸価格     | ○     |
| retail_price    | bigint |    | 小売価格     | ○     |
| remark   | string |    | 備考       | ○     |

### Image テーブル
| フィールド名 | データ型 | キー | 説明       | null許容 |
|----------|-------|----|----------|-------|
| id       | bigint | PK | 画像ID | ×     |
| image    | string |    | 画像       | ×     |

### Template テーブル
| フィールド名 | データ型 | キー | 説明       | null許容 |
|----------|-------|----|----------|-------|
| id       | bigint | PK | テンプレートID | ×     |
| content  | string |    | 内容       | ×     |

### Catalog_section テーブル
| フィールド名   | データ型 | キー | 説明       | null許容 |
|------------|-------|----|----------|-------|
| id         | bigint | PK | ID       | ×     |
| template_id | references | FK | テンプレートID | ×     |
| catalog_id  | references | FK | カタログID   | ×     |
| product_id  | references | FK | 製品ID   | ×     |
| page_number  | bigint |  | ページNo   | ×     |
| page_place_number  | bigint |  | ページプレイスNo   | ×     |