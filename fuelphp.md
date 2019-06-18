# FuelPHP

# migration(マイグレーション)


## コマンド

```
$ php oil refine migrate # 最新バージョンまでUP
$ php oil refine migrate:up # 1バージョンUP
$ php oil refine migrate:down # 1バージョンdown
$ php oil refine migrate:up --version=10 # バージョン10までUP,現在が7だと8,9,10と実行
```

## マイグーレションのバージョンの管理

`config/migrations.php` の `table` で設定したテーブル（初期値はmigration）で管理


# ORM

## find, join

```
$mdl_xxxs = Model_XXX::find('all', [
    'related' => [
        'address' => [
            'join_type' => 'inner', // INNER JOIN
            'join_on' => [
                ['enabled', DB::expr(1)]
            ],
            'related' => [
                'order_detail' => [
                    'join_type' => 'left outer', // LEFT OUTRE JOIN
                    'join_on' => [
                        ['enabled', DB::expr(1)],
                        ['id', DB::expr($order_detail_id)],
                    ],
                ],
            ],
        ],
    ],
    'where' => [
        ['id' => (id)],
        ['enabled' => 1],
    ],
]);
```