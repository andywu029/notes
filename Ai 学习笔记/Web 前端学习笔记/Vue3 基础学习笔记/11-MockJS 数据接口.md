# 基础知识

&emsp;&emsp;在前后端分离开发的项目中，前端和后端人员都是根据 `API` 文档进行项目开发的，不应该直接相互依赖，前端人员不应该等待后端开发好接口后再进行测试。既然不依赖后端接口，此时就可以通过模拟数据生成器，通过一定规则 （`API` 文档）生成模拟数据接口，提供给前端人员进行测试。

&emsp;&emsp;`MockJS` 就是用于生成随机数据、拦截 Ajax 请求的，通过拦截 Ajax 请求，根据数据模板生成并返回模拟数据，让前端独立于后端进行开发，帮助编写单元测试。`MockJS` 的使用过程如下：

1. 首先创建 <font color=brown>***mockjs-demo***</font> 的目录，通过命令行提示符窗口进入到该文件夹，执行 <font color=green>***npm init -y***</font> 命令
2. 通过 <font color=green>***npm install mockjs***</font> 命令安装 `Mock.js`
3. 创建 <font color=brown>***demo1.js***</font> 文件，代码如下：

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList|4': [
        {
            'id': 1,
            "name": "Tom"
        }
    ]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

4. 执行 <font color=green>***node demo1.js***</font>  命令，得到的结果如下：

```json
{
  "memberList": [
    {
      "id": 1,
      "name": "Tom"
    },
    {
      "id": 1,
      "name": "Tom"
    },
    {
      "id": 1,
      "name": "Tom"
    },
    {
      "id": 1,
      "name": "Tom"
    }
  ]
}
```

&emsp;&emsp;`Mock.js` 的语法规范包括两个部分：<font color=red>**数据模板定义规范（Data Template Definition，DTD）和 数据占位符定义规范（Data Placeholder Definition，DPD）**</font>。

# 数据模板定义规范 DTD

&emsp;&emsp;数据模板中的每个属性由 3 部分构成：<font color=red>**属性名、生成规则、属性值**</font>：

```javascript
// '属性名|生成规则': 属性值
'name|rule': value
```

+ 属性名和生成规则之间用竖线 `|` 分隔
+ 生成规则是可选的，生成规则有 7 种格式
  + <font color=red>'name|min-max': value</font>
  + <font color=red>'name|count': value</font>
  + <font color=red>'name|min-max.dmin-dmax':  value</font>
  + <font color=red>'name|min-max.dcount':  value</font>
  + <font color=red>'name|count.dmin-dmax':  value</font>
  + <font color=red>'name|count.dcount':  value</font>
  + <font color=red>'name|+step':  value</font>
+ 生成规则的含义需要依赖属性值的类型才能确定
+ 属性值中可以含有 `@` 占位符
+ 属性值指定了最终值的初始值和类型

## 属性值是字符串

+ <font color=orchid>**'name|count': string**</font> 通过重复 `string` 生成一个字符串，重复次数等于 `count`
+ <font color=orchid>**'name|min-max': string**</font> 通过重复 `string` 生成一个字符串，重复次数大于等于 `min`，小于等于 `max`

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList|4': [
        {
            "name|1-3": "T", // 随机生成 1 到 3 个重复的 'T'
            'phone|11': '8'  // 生成 11 个 8
        }
    ]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "memberList": [
    {
      "name": "TT",
      "phone": "88888888888"
    },
    {
      "name": "TTT",        
      "phone": "88888888888"
    },
    {
      "name": "TT",
      "phone": "88888888888"
    },
    {
      "name": "T",
      "phone": "88888888888"
    }
  ]
}
```

## 属性值是数字

+ <font color=orchid>**'name|+1': number**</font> 属性值自动加 1，初始值为 `number`
+ <font color=orchid>**'name|min-max': number**</font> 生成一个大于等于 `min`、小于等于 `max` 的整数，属性值 `number` 只是用来确定类型
+ <font color=orchid>**'name|min-max.dmin-dmax': number**</font> 生成一个浮点数，整数部分大于等于 `min`、小于等于 `max` ，小数部分保留 `dmin` 到 `dmax` 位

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList|4': [
        {
            'id|+1': 1, // 自增 +1
            "age|1-100": 1, // 随机生成数字 1 到 100
            'salary|1000-10000.1-2': 0  // 随机生成数字，整数部分 1000 - 10000，小数部分 1-2 位
        }
    ]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "memberList": [
    {
      "id": 1,
      "age": 21,
      "salary": 9897.6
    },
    {
      "id": 2,
      "age": 38,
      "salary": 5064.12
    },
    {
      "id": 3,
      "age": 68,
      "salary": 2152.78
    },
    {
      "id": 4,
      "age": 62,
      "salary": 5845.8
    }
  ]
}
```

## 属性值是布尔型

+ <font color=orchid>**'name|1': boolean**</font> 随机生成一个布尔值，值为 `true` 的概率是 `1/2`
+ <font color=orchid>**'name|min-max': value**</font> 随机生成一个布尔值，值为 `value` 的概率是 `min / (min + max)`

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList|4': [
        {
            'status|1': true, // 随机生成布尔值，true 和 false 概率都是 1/2
            'status2|1-3': true // 随机生成布尔值，true 概率都是 1/4， false 是3/4
        }
    ]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "memberList": [
    {
      "status": true,
      "status2": false
    },
    {
      "status": false,
      "status2": false
    },
    {
      "status": true,
      "status2": false
    },
    {
      "status": false,
      "status2": false
    }
  ]
}
```

## 属性值是对象

+ <font color=orchid>**'name|count': object**</font> 从属性值 `object` 中随机选取 `count` 个属性
+ <font color=orchid>**'name|min-max': object**</font> 从属性值 `object` 中随机选取 `min` 到 `max` 个属性

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList|4': [
        {
            'order|2': { id: 1, name: '订单1', price: 68.8 },     // 随机取对象中的2个属性
            'order2|2-3': { id: 1, name: '洗发水', price: 68.8 }, //对象中的2到3个属性
        }
    ]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "memberList": [
    {
      "order": {
        "price": 68.8,
        "name": "订单1"
      },
      "order2": {
        "name": "洗发水",
        "price": 68.8,
        "id": 1
      }
    },
    {
      "order": {
        "id": 1,
        "name": "订单1"
      },
      "order2": {
        "name": "洗发水",
        "price": 68.8
      }
    },
    {
      "order": {
        "name": "订单1",
        "price": 68.8
      },
      "order2": {
        "name": "洗发水",
        "id": 1
      }
    },
    {
      "order": {
        "name": "订单1",
        "id": 1
      },
      "order2": {
        "id": 1,
        "name": "洗发水",
        "price": 68.8
      }
    }
  ]
}
```

## 属性值是数组

+ <font color=orchid>**'name|min-max': array**</font> 通过重复属性值 `array` 生成一个新数组，重复次数大于等于 `min`，小于等于 `max`
+ <font color=orchid>**'name|count': array**</font> 通过重复属性值 `array` 生成一个新数组，重复次数等于 `count`

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList1|4': [{
        'id|+1': 1
    }],
    'memberList2|1-4': [{
        'id|+1': 1
    }]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "memberList1": [
    {
      "id": 1
    },
    {
      "id": 2
    },
    {
      "id": 3
    },
    {
      "id": 4
    }
  ],
  "memberList2": [
    {
      "id": 1
    },
    {
      "id": 2
    }
  ]
}
```

## 值是正则表达式

&emsp;&emsp;<font color=orchid>**'name': regexp**</font>  根据正则表达式 `regexp` 反向生成可以匹配它的字符串，用于生成自定义格式的字符串：

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'memberList|4': [{
        'idCard': /\d{15}|\d{18}/ // 随机生成身份证号, 注意:正则表达式没有单引号 ''
    }]
})

// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

> <font color=orange>**注意：**</font>`regexp` 是没有引号的。

&emsp;&emsp;效果如下：

```json
{
  "memberList": [
    {
      "idCard": "021506321420838471"
    },
    {
      "idCard": "936393195854380563"
    },
    {
      "idCard": "623865152377895"
    },
    {
      "idCard": "580465646578214"
    }
  ]
}
```

# 数据占位符定义规范 DPD

&emsp;&emsp;<font color=green>***Mock.Random***</font> 是一个工具类，用于生成各种随机数据。`Mock.Random` 类中的方法在数据模板中称为 <font color=red>占位符</font>，书写格式为 <font color=green>***@占位符(参数 [, 参数])***</font>，占位符的格式为：

```javascript
'属性名': @占位符
```

&emsp;&emsp;`Mock.Random` 类中提供的完整方法（占位符）如下：

| Type（类型）  | Method（占位符）                                             |
| ------------- | ------------------------------------------------------------ |
| Basic         | `boolean`、`natural`（自然数，大于等于 `0` 的整数）、`integer`、 `float`、`character`、`string`、`range` （整型数组） |
| Date          | `date`（年月日）、`time`（时分秒）、`datetime`（年月日时分秒） |
| Image         | `image`、`dataImage`                                         |
| Color         | `color`                                                      |
| Text          | `paragraph`、`sentence`、`word`、`title`、`cparagraph`、`csentence`、`cword`、`ctitle` |
| Name          | `first`、`last`、`name`、`cfirst`、`clast`、`cname`          |
| Web           | `url`、`domain`、`email`、`ip`、`tld`                        |
| Address       | `area`、`region`                                             |
| Helper        | `capitalize`、`upper`、`lower`、`pick`、`shuffle`            |
| Miscellaneous | `guid`、`id`                                                 |

## 基本占位符

&emsp;&emsp;随机生成基本数据类型的数据，常用的有 `natural/integer/string/float/boolean`：

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'id|+1': 1,
        'name': '@string',
        'price': '@float',
        'status': '@boolean',
    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "id": 1,
      "name": "UWFm",
      "price": -3620732514376716.5,
      "status": false
    },
    {
      "id": 2,
      "name": "E(7q",
      "price": 4753644518265585,
      "status": false
    },
    {
      "id": 3,
      "name": "Y7I",
      "price": 8875194015190760,
      "status": true
    }
  ]
}
```

## 日期占位符

&emsp;&emsp;随机生成日期类型的数据，常用的有 `date/date(format)` 、`time/time(format)` 和 `datetime/datetime(format)`：

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'birthday': '@date', // 默认 yyyy-MM-dd
        'entryDate': '@date("yyyy/MM/dd")', // 指定日期格式 yyyy/MM/dd
        'createDate': '@datetime', // 默认 yyyy-MM-dd HH:mm:ss
        'updateDate': '@datetime("yyyy/MM/dd HH:mm:ss")' //默认yyyy/MM/dd HH:mm:ss
    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "birthday": "1984-11-23",
      "entryDate": "2021/01/25",
      "createDate": "2010-02-03 15:56:12",
      "updateDate": "2004/09/07 20:17:27"
    },
    {
      "birthday": "2020-02-01",
      "entryDate": "1997/08/25",
      "createDate": "1999-09-05 04:03:38",
      "updateDate": "2006/06/18 06:34:02"
    },
    {
      "birthday": "1993-02-09",
      "entryDate": "1992/04/12",
      "createDate": "2006-09-11 21:48:34",
      "updateDate": "1979/05/22 02:39:40"
    }
  ]
}
```

## 图像占位符

&emsp;&emsp;随机生成图片地址，生成的浏览器可以打开，常用的有 `image`：

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'pic': '@image'
    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "pic": "http://dummyimage.com/160x600"
    },
    {
      "pic": "http://dummyimage.com/720x300"
    },
    {
      "pic": "http://dummyimage.com/120x60"
    }
  ]
}
```

## 文本占位符

&emsp;&emsp;随机生成一段文本，常用的有：

+ `ctitle` 随机生成一句中文标题
+ `csentence(mix?, max?)` 随机生成一段中文文本

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'title': '@ctitle(3, 6)', // 中文标题(3到6个字)
        'content': '@csentence(8, 12)', // 一段中文文本(8到12个字)
    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "title": "消点所",
      "content": "同住才专美收压标。"
    },
    {
      "title": "位海理何",
      "content": "因部院查生常易风值成。"
    },
    {
      "title": "类族物引",
      "content": "果义容委便十科少大切青时。"
    }
  ]
}
```

## 名称占位符

&emsp;&emsp;随机生成名称，常见的占位符： 

+ `first` 英文名
+ `last` 英文姓
+ `name` 英文姓名
+ `cfirst` 中文名
+ `clast` 中文姓
+ `cname` 中文姓名

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'first': '@cfirst', // 中文姓
        'last': '@last', // 英文姓
        'name': '@cname' // 中文姓名
    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "first": "吕",
      "last": "Jackson",
      "name": "文娟"
    },
    {
      "first": "毛",
      "last": "Anderson",
      "name": "王敏"
    },
    {
      "first": "谢",
      "last": "Perez",
      "name": "秦杰"
    }
  ]
}
```

## 网络占位符

&emsp;&emsp;可随机生成 `URL`、域名、`IP` 地址和邮件地址，常见的有：

+ `url(protocol?, host?)` 生成 URL
  + `protocol`：协议（如：http）
  + `host`：域名和端口号（如：mengxuegu.com）
+ `domain` 生成域名
+ `ip` 生成 IP 地址
+ `email` 生成邮件地址

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'url': '@url("http", "mengxuegu.com")', // URL
        'domain': '@domain', // 域名
        'ip': '@ip', // IP
        'email': '@email' // 邮箱地址
    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "url": "http://mengxuegu.com/ahllr",
      "domain": "ebgtw.ad",
      "ip": "253.118.167.187",
      "email": "e.itxc@uunh.ae"
    },
    {
      "url": "http://mengxuegu.com/tdqnhqtl",
      "domain": "zojd.sn",
      "ip": "152.182.33.181",
      "email": "l.nvlyulup@kwjksio.nl"
    },
    {
      "url": "http://mengxuegu.com/cyhnu",
      "domain": "wxhf.an",
      "ip": "245.219.1.178",
      "email": "e.xspw@wxlbvyhm.zw"
    }
  ]
}
```

## 地址占位符

&emsp;&emsp;随机生成区域、省市县、邮政编码，常见的有：

+ `regison` 区域（如：华南）
+ `country(true)` 省市县
+ `zip` 邮政编码

```javascript
const Mock = require('mockjs');

const data = Mock.mock({
    'empList|3': [{
        'area': '@region', // 区域
        'address': '@county(true)', // 省市县
        'zipCode': '@zip' // 邮政编码

    }]
})


// stringify(数据, 数据转换函数，缩进空格数)
console.log(JSON.stringify(data, null, 2));
```

&emsp;&emsp;效果如下：

```json
{
  "empList": [
    {
      "area": "华南",
      "address": "内蒙古自治区 乌海市 乌达区",
      "zipCode": "023479"
    },
    {
      "area": "华东",
      "address": "西藏自治区 山南地区 加查县",
      "zipCode": "896135"
    },
    {
      "area": "华南",
      "address": "黑龙江省 绥化市 望奎县",
      "zipCode": "831755"
    }
  ]
}
```

