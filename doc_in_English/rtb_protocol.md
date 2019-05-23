# Real-time Bidding Interface Protocol



## Document description

This document is only used when the DSP is interfaced with the AdVlion Trading Platform (ADX) using the API.

## Interface Description

This protocol refers to [OpenRTB 2.5](https://www.iab.com/wp-content/uploads/2016/03/OpenRTB-API-Specification-Version-2-5-FINAL.pdf). This protocol is compatible with the OpenRTB protocol field, and the new fields are reflected in the extension field.

## Instruction

### Communication method and coding

The underlying communication protocol between AdVlion ADX and DSP uses the HTTP protocol. The BidRequest message is sent in POST mode, and the data format is in JSON format. For WinNotice, exposure monitoring and click monitoring, all use GET to send.

### Ad request

The AdRequest request is the entry for the ad slot request ad and is sent by the SSP to ADX at the URL specified in this document.

#### Request head

| Http head parameter | Instruction |
| --- | --- |
| Content-Type | application/json |

#### BiddRequest information

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | string | Yes| Unique request id |
| imp | array of object | Yes |  Ad information |
| app | object | Yes | APP information |
| device | object | Yes | Device Information |
| user | object | No | User Information |

##### Imp information

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | string | Yes | Unique imp id |
| instl | integer | Yes | Ad slot type<br>0- banner<br>1- interstitial<br>2- full Screen<br>3- native |
| tagid | string | Yes | Ad slot id |
| secure | integer | No | Flag to indicate if the impression requires secure HTTPS URL creative urls and landing page url. By default secure is 0.<br>0- http url <br>1- https url |
| banner | object | No | Return when instl=0/1/2 |
| banner.w | integer | Yes |Ad slot width, unit pixel |
| banner.h | integer | Yes |Ad slot height, unit pixel |
| banner.ext | object | Yes | Extended field of banner |
| banner.ext.ctype | integer | Yes | Type of creative allowed to return<br>1- html or image + landing page<br>2- image + landing page |
| native | object | No | Return when instl=3 |
| native.ver | string | Yes | Native version, currently "1.0" |
| native.ext | string | Yes | Extended field of native |
| native.ext.title_len | integer | No | The maximum length of the title.<br>If the field has a value, it means that this ad needs DSP to return the title. |
| native.ext.desc_len | integer | No | The maximum length of the description.<br>If the field has a value,it means that this ad needs DSP to return the description. |
| native.ext.icon | object | No | Icon.<br>If the field has a value,it means that this ad needs DSP to return the icon. |
| native.ext.icon.w | integer | Yes | Icon width, unit pixel |
| native.ext.icon.h | integer | Yes | Icon height, unit pixel |
| native.ext.img | array of object | No | Image<br>If the field has a value,it means that this ad needs DSP to return image.<br>The number of array elements indicates that you need to return several images. |
| native.ext.img.w | integer | Yes | Image width, unit pixel |
| native.ext.img.h | integer | Yes | Image height, unit pixel |

##### App information

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | string | Yes | APP id，it is used to indentify the AdVlion ADX medias |
| name | string | Yes | APP name |
| domain | string | No | APP official website domain |
| ver | string | Yes | APP version number |
| bundle | string | Yes | BundleID or package name |
| paid | integer | No | Whether need to pay for APP download. <br>0- no<br>1- yes<br>2- pay in app |
| keywords | string | No | APP keywords can be separated by comma |
| storeurl | string | No | APP download url in the app store |

##### Device information

| Parameter  | Type | Required | Description |
| --- | --- | --- | --- |
| dnt | integer | No | 0-allow ad tracking；1-prohibit ad tracking |
| ua | string | Yes | User-Agent of mobile device |
| ip | string | Yes | IP adress of client device. It is required when request via S2S. If the request is initiated directly from the client side, the field can be filled in with an empty string. |
| ipv6 | string | No | ipv6 |
| geo | object | Yes | 	Geographic location informatiom |
| geo.lat | float | No | latitude |
| geo.lon | float | No | longitude |
| geo.timestamp | integer | No | Timestamp when obtaining latitude and longitude data |
| geo.country | string | No | Country,using `ISO-3166-1 Alpha-3` |
| geo.region | string | No | Area,using `ISO 3166-2` |
| geo.city | string | No | City,using`http://www.unece.org/cefact/locode/service/location.html` |
| devicetype | integer | Yes | Device type<br>1- mobilephone<br>2- Tablelet PC |
| make | string | Yes | Device manufacturer |
| model | string | Yes | Device model |
| os | string | Yes | Operation system, "iOS" or "Android" |
| osv | string | Yes | Operation system version |
| w | integer | Yes | Device screen width pixel. |
| h | integer | Yes | Device screen height pixel. |
| ppi | float | Yes | Physical pixel density |
| carrier | string | Yes | Mobile device carrier: MCC+MNC. If there is no value of MCC+MNC, the field can be filled in with an empty string. View `http://en.wikipedia.org/wiki/Mobile_Network_Code` |
| language | string | No | Device language settings, using `alpha-2/ISO 639-1` |
| js | integer | Yes | Whether Javascript is supported<br>1-yes<br>0-no |
| connectiontype | integer | Yes | Network connection type<br>1- wifi<br>2- 2G<br>3- 3G<br>4- 4G |
| ext | object | Yes | Extended field |
| ext.orientation | integer | No | Device orientation<br>0- portrait<br>1- landscape |
| ext.imei_md5 | string | No | MD5 of IMEI value<br>Android device required |
| ext.idfa | string | No | IDFA value<br>iOS device required |
| ext.androidid_md5 | string | No | MD5 of AndroidID<br>Android device required |
| ext.mac_md5 | string | No | MD5 of MAC |
| ext.density | float | No | Device screen pixel density |

##### User information

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | string | No | User unique ID |
| yob | integer | No | Year of birth, 4 digits |
| gender | string | No | Gender<br>M- Male<br>F- Female<br>O- Other<br>Null- Unknown |
| geo | object | No | Geographic location informatiom |
| geo.lat | float | No | latitude |
| geo.lon | float | No | longitude |
| geo.timestamp | integer | No | Timestamp when obtaining latitude and longitude data |
| geo.country | string | No | Country,using `ISO-3166-1 Alpha-3` |
| geo.region | string | No | Area,using `ISO 3166-2` |
| geo.city | string | No | City,using `http://www.unece.org/cefact/locode/service/location.html` |


### AdResponse

#### No bid return

When the DSP does not participate in the auction, it is required to return an HTTP status code of 204.

#### BidResponse Field information

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | string | Yes | The unique request id in the corresponding request |
| seatbid | array of object | No | Ad information, if request succeed |

##### seatbid Field information

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| bid | array of object | Yes | Ad information |

###### bid object information
| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| impid | string | Yes | Impression id in ad request |
| price | integer | Yes | Bid in units of points / thousand times |
| cid | string | Yes | Creative id |
| w | integer | No | Advertising material width, in pixels |
| h | integer | No | Advertising material height, in pixels |
| adm | string | Yes | Material, view [ad format](#素材格式)<br>image(HTML): HTML, including [Replaceable macro information](#replaceable-macro-information)<br>image(image and landing page): JSON<br>native: JSON |
| ext | object | Yes | Extended field |
| ext.instl | integer | Yes | Adspace<br>0- banner<br>1- interstitial<br>2- full Screen<br>3- native |
| ext.ctype | integer | Yes | Adm type of banner/interstitial/full Screen<br>1- HTML<br>2- image+landing page |
| ext.interact_type | integer | Yes | Types of click action, <br>1- Visit the website<br>2- Download application |
| ext.ad_logo | string | No | Advertising source LOGO |
| ext.ad_icon | string | No | "ad" character |
| ext.deeplink | string | No | Redirect URL which of deep-link, use 'ldp' if it doesn't work. <br>If the app is installed beforehand, it redirects to the specific page,<br>otherwise it redirects to the normal landing page. |
| ext.imp_t | array of string | No | Impression tracking URL, winnotice can also be placed in this field.including [Replaceable macro information](#replaceable-macro-information) |
| ext.clk_t | array of string | No | Click tracking URL |

#### Creative forms
- image（image+landing page）
```json
{
    // image url
    "url": "",
    // landing page
    "ldp": "",
    // ad title,not required
    "title": "",
    // ad subtitle, not required
    "subtitle": "",
    // ad description, not required
    "desc": ""
}
```

- native
```json
{
    // title,not required
    "title": "",
    // description,not required
    "desc": "",
    // landing page
    "ldp": "",
    // image（single image or three images）
    "img": [
        {
            // image url
            "url": "",
            "w": 300,
            "h": 250
        },
        {
            // image url
            "url": "",
            "w": 300,
            "h": 250
        },
        {
            // image url
            "url": "",
            "w": 300,
            "h": 250
        }
    ],
    // icon,not required
    "icon": {
        // icon url
        "url": "",
        "w": 300,
        "h": 250
    }
}
```

#### Replaceable macro information

##### Price macro

> ${AUCTION_PRICE}

Decryption algorithm please refer to [Price decryption instructions](doc_in_Chinese/price_decryption.md)
