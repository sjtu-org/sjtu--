# 接口

接口全局约定如下：

* 请求方法为`post`,`get`，跨域使用jsonp
* 请求参数风格使用restful（用户标识信息放入header），请求参数和响应参数content-type统一为 `application/json`
* 响应json至少包含3个字段：`code`（请求返回码，约定 '001'请求成功，其他出错），`msg`（非'001'需要带上的错误信息），`data`（返回数据统一作为data的value）
* 字段名称统一使用驼峰命名
* 用户标识信息，服务端用 同腾讯服务器兑换的 openId，session_key 生成一个 `token`


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
        presetCardId: Integer //第一次进入的随机明信片id，用来标识明信片，为空，则认为不是第一次进入，
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
        flowerStates: [
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
        rewardCardId: Integer, //5 张集满，返回的明信片id
        collectedNum: Interger, //当前收集到的小花数
        taskNum: Interger, //每张明信片所需收集的小花数
    }
}
```


## 获取附近的明信片[GET]

```javascript
url:postcard/location

request: {
    latitude: Double, //wgs84 纬度
    longitude: Double // wgs84 经度
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        postcards: [
            locationId: Integer, // 位置id
            distance: Integer, // 距离，单位m
            direction: Integer, // 方向:0,西北;1,西南;2,东北;3,东南
            state: Integer, // 收集状态:0,可收集;1,未收集;2,已收集
            name: String //地点名称:如果不在范围内，返回为未知
        ]
    }
}
```

## 收集附近的明信片[POST]

```javascript
url:postcard/location/{locationId}

request: {
    latitude: Double, //wgs84 纬度
    longitude: Double // wgs84 经度
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        rewardCardUrl: String, //返回的明信片url
        rewardCardId: Integer, //返回的明信片id
    }
}
```



## 明信片寄送[POST]

```javascript
url:postcard/send

request: {
    departmentId: Integer, //学院唯一标识码
    targetName: String, //发送对象 名称
    content: String, //明信片发送内容
    cardId: Integer, //明信片id
    authorName: String, //作者姓名
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
}
```

## 获取感恩墙明信片列表[GET]

```javascript
url:postcard/wall

request: {
    departmentId: Integer, //学院唯一标识码
    pageSize: Integer, //分页大小
    pageNum: Integer, //页面编号
    searchKey: String, //搜索明信片内容和收件人
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
    "pageAmount": 0, //页面总数
    "pageNum": 0, //页号
    "pageSize": 0, //分页大小
    "result": [
      {
        "authorName": "string", //发件人名字
        "cardId": 0, //卡片id
        "cardUrl": "string", //url链接
        "content": "string", //内容
        "departmentId": 0, //部门
        "targetName": "string", //收件人名字
        "used": true //是否使用过，false为未使用过，可以点击，true为已使用过，不可点击
      }
    ],
    "resultAmount": 0 //已收集明信片种类数
    }
}
```



## 获取单个明信片详情[GET]

```javascript
url:postcard/{cardId}

request: {
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
        "authorName": "string", //发件人名字
        "cardId": 0, //卡片id
        "cardUrl": "string", //url链接
        "content": "string", //内容
        "departmentId": 0, //部门
        "targetName": "string", //收件人名字
        "used": true //是否使用过
    }
}
```


## 获取明信片墙列表[GET]

```javascript
url:postcard

request: {
    pageSize: Integer, //分页大小
    pageNum: Integer, //页面编号
}

response: {
    code: String, //请求返回码
    msg: String, //错误信息
    data: {
    "pageAmount": 0, //页面总数
    "pageNum": 0, //页号
    "pageSize": 0, //分页大小
    "result": [
      {
        "authorName": "string", //发件人名字
        "cardId": 0, //卡片id
        "cardUrl": "string", //url链接
        "content": "string", //内容
        "departmentId": 0, //部门
        "targetName": "string", //收件人名字
        "used": true //是否使用过
      }
    ],
    "resultAmount": 0, //数据总数
    "collectedAmount": 0, //已收集明信片张数
    "totalAmount": 0 //所有种类的明信片总数
    }
}
```

```javascript
//departmentId对应表
1	船舶海洋与建筑工程学院
2	机械与动力工程学院
3	电子信息与电气工程学院
4	材料科学与工程学院
5	环境科学与工程学院
6	生物医学工程学院
7	数学科学学院
8	物理与天文学院
9	化学化工学院
10	致远学院
11	生命科学技术学院
12	农业与生物学院
13	医学院
14	药学院
15	安泰经济与管理学院
16	凯原法学院
17	外国语学院
18	人文学院
19	马克思主义学院
20	国际与公共事务学院
21	媒体与传播学院
22	体育系
23	上海交通大学上海高级金融学院
24	设计学院
25	上海交大密西根学院
26	上海交大-巴黎高科卓越工程师学院
27	上海交大-南加州大学文化创意产业学院
28	高等教育研究院
29	系统生物医学研究院
30	人文艺术研究院
31	科学史与科学文化研究院
32	自然科学研究院
33	先进产业技术研究院

```
