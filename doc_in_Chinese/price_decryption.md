# 价格解密示例

- DSP 获取到的结算价格，是经过加密后的结算价格，每个 DSP 有唯一的结算价格解密密钥，并妥善保管。

- 解密代码请参考`https://developers.google.cn/authorized-buyers/rtb/response-guide/decrypt-price?hl=zh-CN`

## 价格密文和解密示例

1. 示例
    - 密文 `QXByckM1a08wemVZVnZMQpw-pnDFNseH30ArRQ==`
    - 明文 `12345`

2. 示例
    - 密文 `U2JobmxuY0lBSjNMT082NiKQ8Yr64pV_Om_Tcw==`
    - 明文 `1000`

3. 示例
    - 密文 `SFNtYmRYUEs3SnNSNTBhMtC8NAzQd5xRLPterw==`
    - 明文 `88`

## 示例中使用的密钥

- i_key `77c589c81cd026c8170bbf7c1a16c857`
- e_key `b8459ad37b619125b7125026d8005f5a`
