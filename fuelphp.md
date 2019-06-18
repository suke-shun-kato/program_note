# FuelPHP

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