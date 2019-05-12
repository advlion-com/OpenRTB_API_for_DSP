# 竞价请求和返回示例

## 横幅

> 开屏、插屏和横幅广告的请求、返回结构类似，不另外举例

### 横幅请求示例

``` json
{
    "id": "req_id",
    "imp": [
        {
            "id": "imp_id",
            "secure": 0,
            "tagid": "10025",
            "banner": {
                "h": 50,
                "ext": {
                    "ctype": 2
                },
                "w": 320
            },
            "instl": 0
        }
    ],
    "app": {
        "paid": 0,
        "id": "10005",
        "bundle": "pkgname",
        "name": "appname",
        "ver": "1.0"
    },
    "device": {
        "ua": "Mozilla/5.0 (Linux; Android 8.0.0; MIX 2 Build/OPR1.170623.027; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/62.0.3202.84 Mobile Safari/537.36",
        "ip": "183.95.251.96",
        "ppi": 340,
        "ext": {
            "orientation": 0,
            "density": 2,
            "imei_md5": "e7c91a114ea0125780f3366f13c3d12b",
            "mac_md5": "0f607264fc6318a92b9e13c65db7cd3c",
            "androidid_md5": "c084b07e317b3c8b8ba2d3937b921025"
        },
        "carrier": "46000",
        "connectiontype": 1,
        "model": "MIX2",
        "w": "1080",
        "make": "Xiaomi",
        "osv": "7.0.0",
        "os": "Android",
        "devicetype": 1,
        "h": "2160",
        "dnt": 0
    },
    "user": {}
}
```

### 横幅返回示例

```json
{
    "id": "req_id",
    "seatbid": [
        {
            "bid": [
                {
                    "cid": "cid",
                    "price": 1000,
                    "ext": {
                        "clk_t": [
                            "http://clkt1",
                            "http://clkt2"
                        ],
                        "imp_t": [
                            "http://impt1?price=${AUCTION_PRICE}",
                            "http://impt2"
                        ],
                        "interact_type": 1,
                        "instl": 0
                    },
                    "adm": "{\"desc\":\"desc\",\"ldp\":\"http:\\/\\/ldpl1\",\"title\":\"title\",\"url\":\"http:\\/\\/imgurl1\"}",
                    "impid": "imp_id"
                }
            ]
        }
    ]
}
```

## 原生

### 原生请求示例

```json
{
    "id": "req_id",
    "imp": [
        {
            "secure": 0,
            "tagid": "20314",
            "native": {
                "ver": "1.0",
                "ext": {
                    "desc_len": 10,
                    "img": [
                        {
                            "h": 250,
                            "w": 300
                        }
                    ],
                    "title_len": 10,
                    "icon": {
                        "h": 75,
                        "w": 75
                    }
                }
            },
            "id": "imp_id",
            "instl": 3
        }
    ],
    "app": {
        "paid": 0,
        "id": "10005",
        "bundle": "pkgname",
        "name": "appname",
        "ver": "1.0"
    },
    "device": {
        "ua": "Mozilla/5.0 (Linux; Android 8.0.0; MIX 2 Build/OPR1.170623.027; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/62.0.3202.84 Mobile Safari/537.36",
        "ip": "183.95.251.96",
        "ppi": 340,
        "ext": {
            "orientation": 0,
            "density": 2,
            "imei_md5": "e7c91a114ea0125780f3366f13c3d12b",
            "mac_md5": "0f607264fc6318a92b9e13c65db7cd3c",
            "androidid_md5": "c084b07e317b3c8b8ba2d3937b921025"
        },
        "carrier": "46000",
        "connectiontype": 1,
        "model": "MIX2",
        "w": "1080",
        "make": "Xiaomi",
        "osv": "7.0.0",
        "os": "Android",
        "devicetype": 1,
        "h": "2160",
        "dnt": 0
    },
    "user": {}
}
```

### 原生返回示例

```json
{
    "id": "req_id",
    "seatbid": [
        {
            "bid": [
                {
                    "cid": "cid",
                    "price": 100000,
                    "ext": {
                        "clk_t": [
                            "http://clkt1",
                            "http://clkt2"
                        ],
                        "imp_t": [
                            "http://impt1?price=${AUCTION_PRICE}",
                            "http://impt2"
                        ],
                        "interact_type": 1,
                        "instl": 3
                    },
                    "adm": "{\"desc\":\"desc\",\"ldp\":\"desc\",\"title\":\"title\",\"img\":[{\"url\":\"http:\\/\\/imgurl1\",\"h\":300,\"w\":300}],\"icon\":{\"url\":\"http:\\/\\/iconurl1\",\"h\":300,\"w\":300}}",
                    "impid": "imp_id"
                }
            ]
        }
    ]
}
```