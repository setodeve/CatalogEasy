

## テーブル定義書

### usersテーブル

| 列名                   | データ型     | Null許容 | その他                         |
|------------------------|--------------|----------|--------------------------------|
| id                     | bigint       | false    | Primary Key                    |
| provider               | string       | false    | Default: "email"               |
| uid                    | string       | false    | Default: ""                    |
| encrypted_password     | string       | false    | Default: ""                    |
| reset_password_token   | string       | true     |                                |
| reset_password_sent_at | datetime     | true     |                                |
| allow_password_change  | boolean      | false    | Default: false                 |
| remember_created_at    | datetime     | true     |                                |
| confirmation_token     | string       | true     |                                |
| confirmed_at           | datetime     | true     |                                |
| confirmation_sent_at   | datetime     | true     |                                |
| unconfirmed_email      | string       | true     |                                |
| name                   | string       | true     |                                |
| nickname               | string       | true     |                                |
| image                  | string       | true     |                                |
| email                  | string       | true     |                                |
| tokens                 | text         | true     |                                |
| created_at             | datetime     | false    |                                |
| updated_at             | datetime     | false    |                                |

### productsテーブル

| 列名         | データ型 | Null許容 | その他         |
|--------------|----------|----------|----------------|
| id           | bigint   | false    | Primary Key    |
| name         | string   | false    |                |
| size         | string   | false    |                |
| trade_price  | bigint   | true     |                |
| retail_price | bigint   | true     |                |
| remark       | string   | true     |                |
| created_at   | datetime | false    |                |
| updated_at   | datetime | false    |                |
| user_id      | bigint   | true     | Foreign Key    |

### catalog_sectionsテーブル

| 列名             | データ型   | Null許容 | その他          |
|------------------|------------|----------|-----------------|
| id               | bigint     | false    | Primary Key     |
| template_id      | bigint     | false    | Foreign Key     |
| catalog_id       | bigint     | false    | Foreign Key     |
| product_id       | bigint     | false    | Foreign Key     |
| page_number      | bigint     | false    |                 |
| page_place_number| bigint     | false    |                 |
| created_at       | datetime   | false    |                 |
| updated_at       | datetime   | false    |                 |

### catalogsテーブル

| 列名       | データ型   | Null許容 | その他          |
|------------|------------|----------|-----------------|
| id         | bigint     | false    | Primary Key     |
| name       | string     | false    |                 |
| user_id    | bigint     | false    | Foreign Key     |
| created_at | datetime   | false    |                 |
| updated_at | datetime   | false    |                 |

### image_assignmentsテーブル

| 列名            | データ型   | Null許容 | その他          |
|-----------------|------------|----------|-----------------|
| id              | bigint     | false    | Primary Key     |
| product_id      | bigint     | false    | Foreign Key     |
| product_image_id| bigint     | false    | Foreign Key     |
| image_key       | string     | true     |                 |
| created_at      | datetime   | false    |                 |
| updated_at      | datetime   | false    |                 |

### product_imagesテーブル

| 列名       | データ型   | Null許容 | その他          |
|------------|------------|----------|-----------------|
| id         | bigint     | false    | Primary Key     |
| created_at | datetime   | false    |                 |
| updated_at | datetime   | false    |                 |
| user_id    | bigint     | true     | Foreign Key     |

### templatesテーブル

| 列名       | データ型   | Null許容 | その他          |
|------------|------------|----------|-----------------|
| id         | bigint     | false    | Primary Key     |
| content    | string     | false    |                 |
| created_at | datetime   | false    |                 |
| updated_at | datetime   | false    |                 |
