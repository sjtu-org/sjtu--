# 接口

接口全局约定如下：

* 请求方法统一为`post`，跨域使用jsonp
* 请求参数风格不使用query（用户标识信息可放入Url string）/resultful，请求参数和响应参数content-type统一为 `application/json`
* 响应json至少包含3个字段：`code`（请求返回码，约定 '001'请求成功，其他出错），`msg`（非'001'需要带上的错误信息），`data`（返回数据统一作为data的value）
* 字段名称统一使用驼峰命名
* 用户标识信息，服务端用 同腾讯服务器兑换的 openId，session_key 生成一个 `accessToken`（可讨论实现）


## 登陆获取`accessToken`[POST]

```javascript
url:user/login

request: {
    loginCode: String // wx.login兑换的code
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        token: String, //用户唯一标识
        presetCardUrl: String //第一次进入的随机明信片url，为空，则认为不是第一次进入，
    }
}
```


## 获取小花状态以及明信片列表[GET]

```javascript
url:flower

request: {
    
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        flowerList: [
            true, //小花是否被采摘
        ],
        collectedNum: Interger, //当前收集到的小花数
        taskNum: Interger, //每张明信片所需收集的小花数
    }
}
```


## 收集小花[POST]

```javascript
url:flower/{flowerId} //小花的id为0~9

request: {

}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        rewardCardUrl: String, //5 张集满，返回的明信片url
    }
}
```


## 获取附近的明信片

```javascript
url:

request: {
    latitude: String, //wgs84 纬度
    longitude: String // wgs84 精度
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        cardList: [
            // String, 明信片 url
        ]
    }
}
```


## 明信片寄送

```javascript
url:

request: {
    departmentCode: String, //学院唯一标识码
    targetName: String, //发送对象 名称
    content: String, //明信片发送内容
    cardUrl: String, //明信片url
    authorName: String, //作者姓名
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
}
```

## 获取感恩墙明信片列表

```javascript
url:

request: {
    departmentCode: String, //学院唯一标识码
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        mailList: [
            {
                uniqueCode: String, // 明信片唯一标识
                targetName: String, //发送对象 名称
                content: String, //明信片发送内容
                cardUrl: String, //明信片url
            }
            ...
        ]
    }
}
```


## 获取明信片墙列表

```javascript
url:

request: {
    
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        cardList: [
            // String, 明信片 url
        ]
    }
}
```


