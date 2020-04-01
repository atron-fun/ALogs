### 接入步骤

1. 申请接入密钥
2. 在测试环境上进行接口测试验证
3. 测试OK后，切换为正式环境，正式上报数据

### 传输协议

推送的数据统一使用<code>UTF-8</code>字符集

接口在推送数据的同时，需要在HTTP头<code>Authorization</code>带上加密串

<code>Authorization = BASE64(HMAC_SHA1(json，secret))</code>，<code>json</code> 为上报内容，<code>secret</code>由ALogs对接同学提供

正式地址 <code>https://www.domain.fun/test</code>

测试地址 <code>https://www.domain.fun</code>

- 请求方式：POST

- 请求参数：JSON 

    详见<a href='/#/logs'>日志文档</a>
    
**请求样例**    

```
    POST /game/log/test HTTP/1.1
    Host: https://www.domain.fun
    Accept: */*
    Authorization: igotqpr8SjwcHgXKvZjZvargq0M=
    Connection: -alive
    User-Agent: python-requests/2.11.1
    Accept-Encoding: gzip, deflate
    Content-Length: 194（该长度不是写死的，根据具体body的大小设置）

    {"pay_total": "9999", "event_id": "1", "zone_id": "11", "level": "10", "platform": "1",
     "fight_value": "9999", "openid": "1", "event_time": "1477395250", "vip_level": "3", "appid": "1101070777"}
```

- 返回参数说明

    - code 200:成功，400 鉴权错误, 300 参数校验失败
    - message 说明
    
- JSON返回示例

```javascript
{
   code: 200,
   message: 'ok'
}
```

### FAQ 
