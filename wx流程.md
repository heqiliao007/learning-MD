<https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/Wechat_webpage_authorization.html#4> 

### 微信公众平台注册

个人订阅号---无支付功能

服务号---需要认证企业和支付认证

### 微信授权

#### 业务域名

公众号设置--功能设置---业务域名

该域名下不会有安全提示，1月最多可修改并保持3次

#### JS接口安全域名 

公众号设置--功能设置---JS接口安全域名

调用js-sdk

#### 网页授权域名

公众号设置--功能设置---网页授权域名

授权后回调页面

#### access_token

分为授权access_token和普通access_token

#### 授权流程

- 用户同意授权，获取code
- 通过code换取网页授权access_token
- 拉取用户信息（需scope为snsapi_userinfo）
  - 授权分为静默授权 （取进入页面的用户的openid的 ，以snsapi_base为scope发起的网页授权 ）和拉取用户信息授权

<https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/Wechat_webpage_authorization.html> 

微信客户端中

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect 若提示“该链接无法访问”，请检查参数是否填写错误，是否拥有scope参数对应的授权作用域权限。
```

 APPID

REDIRECT_URI 重定向---》授权回调域名配置

公众号设置-测试账号---功能服务--网页账号

vue中需要配置代理 设置主机地址 

host 主机地址需要 windows系统下去映射 127.0.0.1  xxx

授权一般是服务端

### web开发者工具

绑定开发者微信号，要先关注

### UnionID机制

同一用户的UnionID是一样的

### JS-SDK调用流程

- 绑定域名
- 引入js文件
- 通过config接口注入权限证配置（接口签名）
- 通过ready接口处理成功验证

<https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html#1> 

### H5前端架构设计

- src - api 请求地址规范

- src - assets

  放js、img、css等

- 组件名字小写规范

  短横线形式---建文件名

- src - env 环境配置

  dev：开发环境

  test：测试地址

  prod：线上地址

![](.\wx-img\2.JPG)

![3](.\wx-img\3.JPG)

- 公共函数 util

  比如这里是获取参数

  search获取query形式地址

  substr(1)截取掉？

  编码*encodeURIComponent()* 函数可把字符串作为 URI 组件进行编码 

  解码*decodeURIComponent

  注意！这里export default{}直接相当于是export 了一个对象，对象里多个function，调用的时候就可以对象.function()

![](.\wx-img\4.JPG)

- 挂载axios

  import Vue from 'vue'

  import axios from 'axios'

  import VueAxios from 'vue-axios'

  Vue.use(VueAxios, axios)

   VueAxios 是帮助axios 挂载的

  挂载后其他组件可以直接使用this.$http

![](.\wx-img\5.JPG)

### H5接入微信

定义请求地址

![](.\wx-img\1.JPG)

微信授权、注入openId

后端注入openId，前端获取

获取签名信息配置config

后端配置签名

```
wx.config({
  debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
  appId: '', // 必填，公众号的唯一标识
  timestamp: , // 必填，生成签名的时间戳
  nonceStr: '', // 必填，生成签名的随机串
  signature: '',// 必填，签名
  jsApiList: [] // 必填，需要使用的JS接口列表
});
```

前端

![](.\wx-img\6.JPG)

注意hash值不能要，要截取

![](.\wx-img\8.JPG)

上面this.$http.get().then(response =>{})

定义分享公共信息

<https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html#10> 

即将废弃 `wx.onMenuShareTimeline`、`wx.onMenuShareAppMessage`、`wx.onMenuShareQQ`、`wx.onMenuShareQZone`接口

新版本`wx.updateAppMessageShareData`、`wx.updateTimelineShareData` 

![](.\wx-img\9.JPG)

再通过wx.ready(function () {})去调用

![](.\wx-img\7.JPG)

#### 支付功能

##### 3.1唤起支付

```
Type:post
url: http://szgk.sz-water.com.cn/payservice/pay/wxPayInfo
postJson:
 {
    	"channelId":"3",//3:微信公众号
    	"contactId":"U001",//用户id
        "accountId":"U001_wx001",//openid
        "payType":"WxPay",
        "amount":"2",
        "orderTitle":"2019年1月水费账单",
        "details":[
  		 {"customerId":"4520456922","costMonth":"201812","amount":"1"}，
   		{"customerId":"4520456923","costMonth":"201812","amount":"1"}
	]
}
ps：上面的amount是总金额
下面的是每个用户编号的金额
contactId实际是通过接口36获取到的账户id
```

  

```
方法：微信公众号内调起支付（参数皆为返回payInfo内的参数）
WeixinJSBridge.invoke('getBrandWCPayRequest',{
    "appId" : "wx2421b1c4370ec43b", 
    "timeStamp":"1395712654", 
    "nonceStr" : "e61463f8efa94090b1f366cccfbbb444", 
    "package" : "prepay_id=u802345jgfjsdfgsdg888",
    "signType" : "XXX", 
    "paySign" : "70EA570631E4BB79628FBCA90534C63FF7FADD89" 
    },function(res){
 if(res.err_msg == "get_brand_wcpay_request:ok" ) {
    // 使用以上方式判断前端返回,微信团队郑重提示：res.err_msg 将在用户支付成功后返回ok，但并不保证它绝对可靠
    }
});

```



```
返回结果json:
{ 
    "message": null, 
    "statusCode": 0, 
    "content": { 
        "qrcode": null, 
        "paymentSn": "119ae5ed99fe4f028f149709635647cb", 
        "payInfo": "			   {\"appId\":\"wx1f87d44db95cba7a\",\"timeStamp\":\"1554704643798\",\"status\":\"0\",\"signType\"
        :\"MD5\",\"package\":\"prepay_id=wx081424037505877e852927652862187560\",
        \"callback_url\":null,\"nonceStr\":\"1554704643798\",
        \"paySign\":\"828BD0FC76DBA2B8A5A9F6CC8B3FC6B9\"}"
    }, 
    "succeed": true
}

```

##### 3.2支付状态

假如接口地址为：http://szgk.sz-water.com.cn/payservice/pay/payStatus 

入参：取上个接口的message.content.paymentSn

4种状态逻辑是：
（1）支付成功后点击返回跳到支付中，逻辑代码写在2.1 res.err_msg中
（2）目前约定的时间好像是一分钟，如果Timer刷了一分钟没有刷到那个接口返回状态为success的数据，就是网络繁忙
（3）如果failAmount=0则可以记为成功 
（4）failAmount>0则把failList里面的用户编号取出来显示，具体规则依据UI

##### 3.3实例代码

```
//支付接口
	function fnGetWXPayInfo(){
		//address,paydate,userID,zo_money
		var url_prefix = "http://szgk.sz-water.com.cn/payservice/";
		var paydateStr = String(billDate).slice(0,4)+'年'+String(billDate).slice(4)+'月';
		//测试amount暂为1分 测试通过后取amount_money
		var amount_money = needPay*100;
		var payData = {
			"channelId":"3",//3:微信公众号
			"contactId":userID,
			"accountId":"olfjEjupsHr-OsExEk-hiJIV-m9c",
			"payType":"WxPay",
			"amount":1,
			"orderTitle":paydateStr+"水费",
			"details":[
				{
					"customerId":customerCode,
					"costMonth":billDate,
					"amount":1
				}
			]
		};
		console.log(payData,amount_money)
		var url = url_prefix + "pay/wxPayInfo";
		$.ajax({
	        url: url,
	        type: 'POST',
	        timeout:180000,
	        data: JSON.stringify(payData),
	        cache: false,
	        dataType: 'json',
	        contentType: 'application/json;charset=UTF-8',
	        processData: false,
	        success: function (response) {
	        	if(response!=null){
	        		if(response.succeed==true){
	        			//var token = response.content;
	            		//var paymentSN = token.paymentSN;
	            		//$('#txt_content_wx').html(JSON.stringify(response));
	            		var payInfo = response.content.payInfo;
	            		var paymentSn = response.content.paymentSn;
	            		payInfo = eval('(' + payInfo + ')');
	        			WeixinJSBridge.invoke('getBrandWCPayRequest',{
	        				"appId" : payInfo.appId,
	        				"timeStamp": payInfo.timeStamp,
	        				"nonceStr" : payInfo.nonceStr,
	        				"package" : payInfo.package,
	        				"signType" : payInfo.signType,
	        				"paySign" : payInfo.paySign
	    				},function(res){
	        				if(res.err_msg == "get_brand_wcpay_request:ok" ) {
	        					//	使用以上方式判断前端返回,微信团队郑重提示：res.err_msg 将在用户支付成功后返回ok，但并不保证它绝对可靠。
	        					localStorage.setItem('paymentSn', paymentSn);
	        					console.log("交费成功！");
								window.location.href = 'check-pay-ing.html?billmonth='+window.btoa(paydate)+'&customerCode='+window.btoa(address);
	        				}
	        			});
	        		}
	            	else{
	            		alert("错误");
	            		//$('#txt_content_wx').html(JSON.stringify(response.message));
	            	}
	        	}
	        },
			error:function(){
				console.log("error");
			}
		});
	};
```

```

<script>
		//跳转到支付中后状态处理
		const url_prefix = "http://szgk.sz-water.com.cn/payservice/";
		//取出paymentSn
		let paymentSn = localStorage.getItem('paymentSn');
		// 测试paymentSn c3bfa90e0ad4435f8f786eafd8f21f8d
		let obj = {"paymentSn":paymentSn};
		//stateSuc判断状态
		let stateSuc = false;
		//获取customerCode和billmonth
		let customerCode = window.atob(common_fn_getCode("customerCode"));
		let billmonth = window.atob(common_fn_getCode("billmonth"));
		function state(){
			$.ajax({
		        url: url_prefix + "pay/payStatus",
		        type: 'POST',
		        timeout:180000,
		        data: JSON.stringify(obj),
		        cache: false,
		        dataType: 'json',
		        contentType: 'application/json;charset=UTF-8',
		        processData: false,
		        success: function (res) {
		        	if(res.statusCode == 0){
		        		console.log(res)
		        		//给stateSuc赋值
		        		stateSuc = res.succeed;
		        		let data = JSON.parse(res.content)
						//console.log(stateSuc)
						//判断失败
						let failAmount = data.failAmount;
						//失败编号
						let failListNum='';
						failListNum += data.failList.map( item => {
						    return item.customerid;
						});
						console.log(failListNum)
						//成功
		        		if(failAmount == 0){
							//如果要跳转了  即可删除paymenrSn
		    				localStorage.removeItem('paymentSn');
							//	window.location.href = 'check-pay-certificate.html?billmonth=' + billmonth + '&customerCode=' + customerCode;
		    				window.location.href = 'check-pay-certificate.html?billmonth=' + window.btoa(billmonth) + '&customerCode=' + window.btoa(customerCode);
						//失败
				        }else if(failAmount > 0){
				        	//跳转失败页面 url做处理
		    				localStorage.removeItem('paymentSn');
							window.location.href = 'check-pay-fail.html?failListNum='+window.btoa(failListNum);
				        }
		        	}else{
						console.log('error');
		        	}
		        },
				error:function(request){
					console.log(request);
				}
		    })
		}
		//等待效果+刷新stateSuc状态
		var timerWait = setInterval(() => {
			state();
		}, 3000)
		//执行一次 stateSuc仍然为false的时候将跳转繁忙
		var timerBusy = setTimeout(() => {
		//	console.log(stateSuc)
			//清除定时器
			clearInterval(timerWait)
		    if(!stateSuc){
		    	localStorage.removeItem('paymentSn');
				window.location.href = 'check-pay-busy.html';
		    }
		}, 60000)
	</script>
```

