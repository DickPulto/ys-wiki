# 微信 JSSDK 组件

> 本组件由 Yami 开发

[[toc]]

---

- 示例：https://yami.h5.yscase.com/yami_share/demo.html （浏览器加载，请在微信内打开）
- 示例：https://yami.h5.yscase.com/npm/dist/ （Vue框架使用，请在微信内打开）
- 微信JSSDK官方文档：https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html

---

## 安装

### 用 CDN 方式
```javascript
<script src="//res.wx.qq.com/open/js/jweixin-1.6.0.js"></script>
<script src="//common.yscase.com/js/WechatJSSDK.js"></script>
```

### 用 NPM 方式
> _请注意 NPM 微信JSSDK 版本是1.4.0_
```javascript
yarn add ys-wechat-jssdk # 或者用 npm i ys-wechat-jssdk
import WechatJSSDK from 'ys-wechat-jssdk'
```

---

## 使用


构造方法对象参数
字段名 | 类型 | 名称  | 必须 | 默认值 | 说明
---|---|---|---|---|---
client_id | string | 公众号ID | 否 | 9545d869-95eb-47ad-8667-98b5541d4af2 | 默认为语铄公众号
url | string | 当前网页的URL | 否 | location.href | 
debug | bool | 开启调试模式 | 否 | false | 
onSDKReady | function | 签名验证成功试回调函数 | 否 | {} | 
onSDKError | function | 签名验证失败时回调函数 | 否 | {} | 
title | string | 分享标题 | 否 | 网页TITLE |  
desc | string | 分享描述 | 否 | 空 |  
link | string | 分享URL | 否 | location.href |  
imgUrl | string | 分享图标 | 否 | body 标签内第一张图 |  


### 使用示例1：全部使用默认值 

```javascript
let jssdk = new WechatJSSDK();
```

### 使用示例2：自定义参数
```
// 示例2 使用自定义参数且后续不需要用到微信JSSDK功能
new WechatJSSDK({
    client_id: '1dfcc42c-f073-40e6-9311-1466319d7d4d',
    url: 'https://yami.h5.yscase.com/yami_share/demo.html',
    debug: true,
    onSDKReady: function(){
        document.getElementById('bgm').play();
    },
    title: '标题文案',
    desc: '描述',
    link: 'https://yami.h5.yscase.com/yami_share/demo.html',
    imgUrl: 'https://yami.h5.yscase.com/yami_share/share.jpg'
});

 
```

## 接口列表

### 1. share - 微信分享

对象参数
- title：分享标题，默认网页 TITLE 标签内容
- desc： 分享描述，默认为空
- link：分享的URL，默认是引入组件的页面地址
- imgUrl: 分享图标，默认是页面 body 内第一张图

```javascript
jssdk.share({
    title: '标题文案',
    desc: '描述',
    link: 'https://yami.h5.yscase.com/yami_share/demo.html',
    imgUrl: 'https://yami.h5.yscase.com/yami_share/share.jpg',
    complete: function() {
      // 分享完做什么
    }
});
```

### 2. 隐藏、显示所有非基础按钮接口

```javascript
// 隐藏
jssdk.hideAllNonBaseMenuItem();

// 显示
jssdk.showAllNonBaseMenuItem();
```


### 3. scan - 调用微信扫一扫

对象参数
- needResult：可选，默认为1，扫描结果由微信处理，1则直接返回扫描结果，
- scanType： 可选，默认为：["qrCode","barCode"]，可以指定扫二维码还是一维码，默认二者都有
- success：成功时的回调函数，支持

```javascript
jssdk.scan({
    success: function (res) {
        console.log('success', res.resultStr);
    }
});
```

### 4. getNetworkType - 获取当前访问用户使用的网络类型

对象参数
- success：成功时的回调函数，支持
```javascript
jssdk.getNetworkType({
    success: function (res) {
        console.log(res.networkType);
    }
});
```

### 5. openLocation - 用腾讯地图打开指定位置

对象参数
- latitude：纬度，浮点数，范围为90 ~ -90
- longitude： 经度，浮点数，范围为180 ~ -180
- name: 位置名
- address: 地址
- scale：地图初始化时缩放比例
- infoUrl：在查看位置界面底部显示的超链接,可点击跳转

```javascript
jssdk.openLocation({
    latitude: 29.652852, // 纬度，浮点数，范围为90 ~ -90
    longitude: 91.118288, // 经度，浮点数，范围为180 ~ -180。
    name: '布达拉宫', // 位置名
    address: '西藏自治区拉萨市城关区北京中路35号', // 地址详情说明
    scale: 14, // 地图缩放级别,整形值,范围从1~28。默认为最大
    infoUrl: 'http://www.potalapalace.cn/' // 在查看位置界面底部显示的超链接,可点击跳转
})
```

### 5. getLocation - 获取当前访问者的地理坐标信息

对象参数
- type：可选，默认为wgs84的gps坐标，如果要返回直接给openLocation用的火星坐标，可传入'gcj02'
- success： 获取成功时回调

```javascript
jssdk.getLocation({
    success: function (res) {
        console.log('维度：', res.latitude);
        console.log('经度：', res.longitude);
        console.log('速度：', res.speed);
        console.log('位置精度：', res.accuracy);
    }
})
```

### 6. pay - 微信支付

_请注意：调用支付方法之前需要和后端对接生成订单信息才可以使用_

对象参数
- timestamp：支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
- nonceStr： 支付签名随机串，不长于 32 位
- package: 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
- signType: 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
- paySign：支付签名
- success：支付成功后的回调函数

```javascript
jssdk.pay({
    timestamp: 0, // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
    nonceStr: '', // 支付签名随机串，不长于 32 位
    package: '', // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
    signType: '', // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
    paySign: '', // 支付签名
    success: function (res) {
        // 支付成功后的回调函数
    }
});
```
