# 实时竞价接口协议



## 文档说明

此文档仅供 DSP 与 AdVlion 交易平台（ADX） 使用 API 方式对接时使用。

## 接口说明

本协议参考 [OpenRTB 2.5](https://www.iab.com/wp-content/uploads/2016/03/OpenRTB-API-Specification-Version-2-5-FINAL.pdf)，兼容 OpenRTB 协议字段，新增的字段在扩展字段中体现。

## 接入说明

### 通信方式及编码

AdVlion ADX 与 DSP 之间的基础通信协议采用 HTTP 协议。发送 BidRequest 消息采用 POST 方式，数据格式使用 JSON 格式。对于 WinNotice、曝光监测和点击监测，都使用 GET 方式发送。

### 广告请求

AdRequest 请求是广告位请求广告的入口，由 SSP 按本文档中规定 URL 向 ADX 发送。

#### 请求头

| http 头信息段 | 说明 |
| --- | --- |
| Content-Type | application/json |

#### BiddRequest 字段信息

| 字段名称 | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| id | string | 是| 唯一请求id |
| imp | array of object | 是 | 请求广告的信息 |
| app | object | 是 | APP 信息 |
| device | object | 是 | 设备信息 |
| user | object | 否 | 用户信息 |

##### imp 对象信息

| 字段名称 | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| id | string | 是 | 唯一曝光id |
| instl | integer | 是 | 广告位类型<br>0- banner<br>1- 插屏<br>2- 开屏<br>3- 原生 |
| tagid | string | 是 | 广告位id |
| secure | integer | 否 | 是否需要 https 链接的标识，默认为 0<br>0- 不需要<br>1- 需要 |
| banner | object | 否 | instl=0/1/2 时返回 |
| banner.w | integer | 是 | 广告位宽度，单位像素 |
| banner.h | integer | 是 | 广告位高度，单位像素 |
| banner.ext | object | 是 | banner 扩展字段 |
| banner.ext.ctype | integer | 是 | 允许返回的创意类型<br>1- html 或 图片+落地页<br>2- 图片+落地页 |
| native | object | 否 | instl=3  时返回 |
| native.ver | string | 是 | 原生版本，目前为 "1.0" |
| native.ext | string | 是 | native 扩展字段 |
| native.ext.title_len | integer | 否 | 标题最大长度。<br>如果该字段有值，则表示本次广告需要 DSP 返回标题 |
| native.ext.desc_len | integer | 否 | 描述最大长度。<br>如果该字段有值，则表示本次广告需要 DSP 返回描述 |
| native.ext.icon | object | 否 | 小图。<br>如果该字段有值，则表示本次广告需要 DSP 返回小图 |
| native.ext.icon.w | integer | 是 | 小图宽度，单位像素 |
| native.ext.icon.h | integer | 是 | 小图高度，单位像素 |
| native.ext.img | array of object | 否 | 大图。<br>如果该字段有值，则表示本次广告需要 DSP 返回大图<br>数组元素数量表示需要返回几个大图 |
| native.ext.img.w | integer | 是 | 大图宽度，单位像素 |
| native.ext.img.h | integer | 是 | 大图高度，单位像素 |

##### app 对象信息

| 字段名称 | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| id | string | 是 | 媒体 ID，标识 AdVlion ADX 媒体的id |
| name | string | 是 | APP 名称 |
| domain | string | 否 | APP 官网域名 |
| ver | string | 是 | APP 版本号 |
| bundle | string | 是 | BundleID 或者包名 |
| paid | integer | 否 | 是否为付费 APP<br>0- 不是<br>1- 是<br>2- 应用内付费 |
| keywords | string | 否 | APP 关键字，可以用英文逗号分隔多个 |
| storeurl | string | 否 | APP 在应用市场的下载地址 |

##### device 对象信息

| 字段名称  | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| dnt | integer | 否 | 0-允许广告追踪；1-不允许广告追踪 |
| ua | string | 是 | 移动设备的 User-Agent |
| ip | string | 是 | 客户端IP地址。如果从客户端直接发起请求，该字段可填空；如果从服务端发起请求，请填写客户端的IP |
| ipv6 | string | 否 | ipv6 |
| geo | object | 是 | 地理位置信息对象 |
| geo.lat | float | 否 | 纬度 |
| geo.lon | float | 否 | 经度 |
| geo.timestamp | integer | 否 | 获取经纬度数据时的时间戳 |
| geo.country | string | 否 | 国家，使用 `ISO-3166-1 Alpha-3` |
| geo.region | string | 否 | 地区，使用 `ISO 3166-2` |
| geo.city | string | 否 | 城市，使用`http://www.unece.org/cefact/locode/service/location.html` |
| devicetype | integer | 是 | 设备类型<br>1- 手机<br>2- 平板 |
| make | string | 是 | 设备制造商 |
| model | string | 是 | 设备型号 |
| os | string | 是 | 设备操作系统，二选一填写 Android 或 iOS |
| osv | string | 是 | 设备操作系统版本号 |
| w | integer | 是 | 设备屏幕分辨率宽，单位为像素 |
| h | integer | 是 | 设备屏幕分辨率高，单位为像素 |
| ppi | float | 是 | 每英寸像素密度 |
| carrier | string | 是 | 设备使用的运营商：MCC+MNC的值。没有则为空字符串。参考`http://en.wikipedia.org/wiki/Mobile_Network_Code` |
| language | string | 否 | 设备的语言设置,使用 `alpha-2/ISO 639-1` |
| js | integer | 是 | 是否支持 Javascript 脚本<br>1-支持<br>0-不支持 |
| connectiontype | integer | 是 | 设备联网类型<br>1- wifi<br>2- 2G<br>3- 3G<br>4- 4G |
| ext | object | 是 | 扩展字段 |
| ext.orientation | integer | 否 | 设备屏幕方向<br>0- 竖向<br>1- 横向 |
| ext.imei_md5 | string | 否 | IMEI 值的 MD5<br>Android 设备必填 |
| ext.idfa | string | 否 | IDFA 值<br>iOS 设备必填 |
| ext.androidid_md5 | string | 否 | AndroidID 的 MD5<br>Android 设备必填 |
| ext.mac_md5 | string | 否 | MAC 的 MD5 |
| ext.density | float | 否 | 设备屏幕像素密度 |

##### user 对象信息

| 字段名称 | 类别 | 必须 | 描述 |
| --- | --- | --- | --- |
| id | string | 否 | 用户唯一 ID |
| yob | integer | 否 | 出生年，4 位数字 |
| gender | string | 否 | 性别<br>M- Male<br>F- Female<br>O- Other<br>Null- Unknown |
| geo | object | 否 | 用户家庭位置 |
| geo.lat | float | 否 | 纬度 |
| geo.lon | float | 否 | 经度 |
| geo.timestamp | integer | 否 | 获取经纬度数据时的时间戳 |
| geo.country | string | 否 | 国家，使用 `ISO-3166-1 Alpha-3` |
| geo.region | string | 否 | 地区，使用 `ISO 3166-2` |
| geo.city | string | 否 | 城市，使用`http://www.unece.org/cefact/locode/service/location.html` |


### 广告返回

#### 无竞价返回

DSP 不参与竞价时，要求返回 HTTP 状态码 204。

#### BidResponse 字段信息

| 字段名称 | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| id | string | 是 | 对应请求中的唯一请求id |
| seatbid | array of object | 否 | 有广告填充时返回，广告信息 |

##### seatbid 对象信息

| 字段名称 | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| bid | array of object | 是 | 广告信息 |

###### bid 对象信息
| 字段名称 | 类型 | 必须 | 描述 |
| --- | --- | --- | --- |
| impid | string | 是 | 对应请求中的曝光id |
| price | integer | 是 | 出价，单位是 分/千次 |
| cid | string | 是 | 创意id，用于标识唯一创意 |
| w | integer | 否 | 广告物料宽度，单位：像素 |
| h | integer | 否 | 广告物料高度，单位：像素 |
| adm | string | 是 | 素材，详情见[素材格式](#素材格式)<br>图片类（HTML）：HTML，可包含[可替换宏信息](#可替换宏信息)<br>图片类（图片+落地页）：JSON<br>原生：JSON |
| ext | object | 是 | 扩展字段 |
| ext.instl | integer | 是 | 对应请求中的广告位类型<br>0- banner<br>1- 插屏<br>2- 开屏<br>3- 原生 |
| ext.ctype | integer | 是 | 横幅/插屏/开屏的创意类型<br>1- HTML<br>2- 图片+落地页 |
| ext.interact_type | integer | 是 | 用户点击行为<br>1- 浏览网页<br>2- 下载应用 |
| ext.ad_logo | string | 否 | 广告来源LOGO图片 |
| ext.ad_icon | string | 否 | "广告"字样图片 |
| ext.deeplink | string | 否 | deeplink<br>如果终端安装了对应的APP，则跳转到deeplink<br>如果没有安装，则跳转普通落地页 |
| ext.imp_t | array of string | 否 | 曝光监测，winnotice 也可以放在该字段中。可包含[可替换宏信息](#可替换宏信息) |
| ext.clk_t | array of string | 否 | 点击监测 |

#### 素材格式
- 图片类（图片+落地页）
```json
{
    // 图片地址
    "url": "",
    // 落地页
    "ldp": "",
    // 广告标题，非必填
    "title": "",
    // 广告副标题，非必填
    "subtitle": "",
    // 广告描述，非必填
    "desc": ""
}
```

- 原生
```json
{
    // 标题，非必填
    "title": "",
    // 描述，非必填
    "desc": "",
    // 落地页
    "ldp": "",
    // 大图（可能有单图或三图）
    "img": [
        {
            // 图片地址
            "url": "",
            "w": 300,
            "h": 250
        },
        {
            // 图地址
            "url": "",
            "w": 300,
            "h": 250
        },
        {
            // 图地址
            "url": "",
            "w": 300,
            "h": 250
        }
    ],
    // 图标，非必填
    "icon": {
        // 图标地址
        "url": "",
        "w": 300,
        "h": 250
    }
}
```

#### 可替换宏信息

##### 价格宏

> ${AUCTION_PRICE}

解密算法请看[价格解密说明](price_decryption.md)
