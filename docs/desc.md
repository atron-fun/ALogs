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

- 请求参数：JSON （单条json放在body里面，注意json拼装，全局字段独立出来，通用字段放到data里面）

    样例：

```json
[
    {
        "body": "{\"appId\": \"1313131\", \"zoneId\": \"1313\", \"version\": \"1.1.0\",\"event\": \"noviceNodeLogs\", \"recordTime\": \"2020-04-03 10:59:45\",\"data\": {\"userId\": \"1\",\"logTime\":\"2018-12-13 00:00:01\"}}"
    },
    {
        "body": "{\"appId\": \"1313131\", \"zoneId\": \"1313\", \"version\": \"1.1.0\",\"event\": \"noviceNodeLogs\", \"recordTime\": \"2020-04-03 10:59:45\",\"data\": {\"userId\": \"1\",\"logTime\":\"2018-12-13 00:00:01\"}}"
    }
]
```

    详见<a href='/#/logs'>日志文档</a>
    
**请求样例**    

java示例：

```java

```

Python示例：

```python
    # -*- coding:utf-8 -*-

    import json
    import requests
    import time
    import base64
    import hmac
    from hashlib import sha1

    if __name__ == '__main__':
        try:
            recordTime=time.strftime("%Y-%m-%d %H:%M:%S", time.localtime(time.time()))
            sceretKey = 'SECRET'
            url='https://www.domain.fun/test'
            event1 = '{"appId": "1313131", "zoneId": "1313", "version": "1.1.0","event": "noviceNodeLogs", "recordTime": "%s","data": {"userId": "1","logTime":"2020-04-07 00:00:01"}}' % (recordTime)
            event2 = '{"appId": "1313131", "zoneId": "1313", "version": "1.1.0","event": "noviceNodeLogs", "recordTime": "%s","data": {"userId": "1","logTime":"2020-04-07 00:00:01"}}' % (recordTime)
            
            payload = json.dumps([
                {"body":event1},
                {"body":event2}
            ])

            # 计算签名
            hmac_code = hmac.new(sceretKey.encode(), payload.encode(), sha1).digest()
            signature = base64.b64encode(hmac_code).decode()
            
            headers = {"Authorization":signature}
            
            print(payload,headers)
            
            result = requests.post(url, data=payload, headers=headers,timeout=15)
            
            if result.status_code == 200:
                print('SUCCESS')
            else:
                print('ERROR')
        
        except Exception as e:
            print(e)


```

Bash示例：

```bash
KEY=$(echo -n '[{"body": "{\"appId\": \"1313131\", \"zoneId\": \"1313\", \"version\": \"1.1.0\",\"event\": \"noviceNodeLogs\", \"recordTime\": \"2020-04-03 10:59:45\",\"data\": {\"userId\": \"1\",\"logTime\":\"2018-12-13 00:00:01\"}}"},{"body": "{\"appId\": \"1313131\", \"zoneId\": \"1313\", \"version\": \"1.1.0\",\"event\": \"noviceNodeLogs\", \"recordTime\": \"2020-04-03 10:59:45\",\"data\": {\"userId\": \"1\",\"logTime\":\"2018-12-13 00:00:01\"}}"}]' | openssl dgst -hmac 'SECRET' -sha1 -binary | base64)

curl -X POST -H "Authorization:$(KEY)" -d'[{"body": "{\"appId\": \"1313131\", \"zoneId\": \"1313\", \"version\": \"1.1.0\",\"event\": \"noviceNodeLogs\", \"recordTime\": \"2020-04-03 10:59:45\",\"data\": {\"userId\": \"1\",\"logTime\":\"2018-12-13 00:00:01\"}}"},{"body": "{\"appId\": \"1313131\", \"zoneId\": \"1313\", \"version\": \"1.1.0\",\"event\": \"noviceNodeLogs\", \"recordTime\": \"2020-04-03 10:59:45\",\"data\": {\"userId\": \"1\",\"logTime\":\"2018-12-13 00:00:01\"}}"}]'  http://www.domain.fun/test

```


- 返回状态说明

    - 返回状态码为200成功，非200为异常。
    

### 接入注意事项

1. 数据合并提交（比如多少条数据提交一次或者多久合并提交一次，单次合并不超过一千条）

2. 单独线程异步提交，不要影响在线玩家，设置 timeout，建议 15s

3. 异常尝试次数，建议 3 次

4. 如果三次接口异常数据可以持久化到本地一个文件，等接口恢复后再读取出数据来提交； （视实现难度，实在不行数据也可以丢弃。三次不通就把数据丢弃，下次继续类似的逻辑）

5. 游戏对于 Alog 的接入，实现参数化可配置（功能开关、上报的表开关，上报频率的配置 （细化到表））


### FAQ 
