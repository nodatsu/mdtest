# ぐんぐん API仕様書 v.0.1

URL https://xxx.herokuapp.com/ (未定)

## 1.ユーザー個人の情報

トップ画面に必要

```
GET /users/:card_number.json
```

応答

```
{
    "role":"student",
    "nick_name":"あおもりはなこ",
    "recent_cell": 20,
    "cell": 26,
    "statuses": [
        {
            "category":"健康",
            "level": 1,
            "recent_experience": 10,
            "experience": 16,
            "next_level_required_experience": 50
        },
        {
            "category":"お友達・あいさつ",
            "level": 1,
            "recent_experience": 8,
            "experience": 12,
            "next_level_required_experience": 50
        }
    ],
    "assigns": [
        {
            "mission_id": 1,
            "category":"健康",
            "level": 2,
            "description":"手を洗う"
        },
        {
            "id": 4,
            "category":"お友達・あいさつ",
            "level": 2,
            "description":"あいさつをする"
        }
    ]
}
```

例外

```
{ "error":"404 Not Found","detail":"user not found with card_number=F25C2" }
```

## 2.ミッションの一覧取得

ミッション小項目設定に必要

```
GET /categories/:category_id.json
```

応答

```
{
    "name": "健康",
    "levels": [
        {
            "value": 1,
            "missions": [
                    { "id": 1, "description": "ああああ" },
                    { "id": 2, "description": "いいいい" },
                    { "id": 3, "description": "うううう" },
                    { "id": 4, "description": "ええええ" },
                    ….
                    { "id": 9, "description": "おおおお" }
            ]
        },
        {
            "value": 2,
            "missions": [
                    { "id": 10, "description": "かかかか" },
                    { "id": 11, "description": "きききき" },
                    { "id": 12, "description": "くくくく" },
                    { "id": 13, "description": "けけけけ" }
            ]
        },
    ]
}
```

## 3.ミッションの設定

```
POST /assigns.json
```

送信データ(HTTP)

```
user_id=3
mission_ids[]=20
mission_ids[]=18
```

```
user_id=3&mission_ids[]=20&mission_ids[]=18
```

送信データ(JSON)

```
{ "user_id":3, "missions_ids": [20, 18] }
```

ヘッダーとして「Content-type: application/json」を先に送る

応答

```
{ "user_id":3, "missions_ids": [20, 18] }
```

例外

user_idに対応したユーザーがいない

```
{"error":"404 Not Found","detail":"user not found with user_id=3"}
```

## 4.ミッションの達成

```
POST /histories.json
```

送信データ(HTTP)

```
user_id=3
mission_ids[]=20
mission_ids[]=18
```

```
user_id=3&mission_ids[]=20&mission_ids[]=18
```

送信データ(JSON)

```
{ "user_id":3, "missions_ids": [20, 18] }
```

ヘッダーとして「Content-type: application/json」を先に送る

応答

```
{ "user_id":3, "missions_ids": [20, 18] }
```

例外

user_idに対応したユーザーがいない

```
{"error":"404 Not Found","detail":"user not found with user_id=3"}
```

user_idに対応したユーザーはいるが、mission_idsにミッションが割り振られていない

```
{"error":"404 Not Found","detail":"mission_ids[]=20 does not assigned with user_id=3"}
```

curlによる送信の場合

```
$ curl -X POST -H "Content-type: application/json" -d "{ user_id: 3, mission_ids: [20, 12] } http://localhost:3000/api/histories.json
```

## 5.ユーザーのすごろく情報(今回分)

ユーザー情報を参照する

## 6.対応したURLが存在しない場合の例外

```
{"error":"404 Not Found","detail":"routing error"}
```
