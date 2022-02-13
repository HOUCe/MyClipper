# uni-app 中如何打开外部应用，如：浏览器、淘宝、AppStore、QQ等 - DCloud问答

source:: chrome-extension://ask.dcloud.net.cn/

我们在开发 App 应用中，经常会遇到打开第三方程序的场景，比如打开手机淘宝、通过第三方浏览器打开一个 url 等等。

App不像网页可以使用http超链接互相跳转，但手机os设计了scheme机制，可以通过特殊的链接互相调起。

比如手机淘宝，其安装后会在手机os中会注册一个scheme协议，`taobao://`。

这种协议还支持参数，比如`taobao://s.taobao.com/search?q=uni-app`启动淘宝并打开搜索页面搜索uni-app。

在uni-app/5+App中，可以通过scheme呼起其他App，也支持给自己的App设置scheme参数。

这个功能小程序并不支持，属于App端的扩展API。

打开外部scheme的API是`plus.runtime.openURL()`。查看文档：[http://www.html5plus.org/doc/zh\_cn/runtime.html](http://www.html5plus.org/doc/zh_cn/runtime.html)

## 打开第三方程序

打开第三方程序，我们需要使用 [runtime](http://www.html5plus.org/doc/zh_cn/runtime.html) 模块,下面我罗列两个相关的方法。其他操作请详读文档。

1.  调用外部浏览器打开指定的URL
    
    复制代码`plus.runtime.openURL( url, errorCB, identity );` 
    
    -   url: ( String ) 必选 要打开的URL地址  
        字符串类型，各平台支持的地址类型存在差异，参考平台URL支持表。
    -   errorCB: ( OpenErrorCallback ) 可选 打开URL地址失败的回调  
        打开指定URL地址失败时回调，并返回失败信息。
    -   identity: ( String ) 可选 指定打开URL地址的程序名称  
        在iOS平台此参数被忽略，在Android平台为程序包名，如果指定的包名不存在，则打开URL地址失败。
    
    复制代码 `<template>  
            <view>  
                <button class="button" type="primary" @click="open(0)">使用第三方程序打开指定URL</button>  
            </view>  
        </template>` 
    
    复制代码`<script>  
    export default {  
        data() {  
            return {  
                url: 'https://uniapp.dcloud.io/'  
            };  
        },  
        onLoad(op) {},  
        methods: {  
            open(types) {  
                    plus.runtime.openURL(this.url, function(res) {  
                        console.log(res);  
                    });  
            }  
        }  
    };  
    </script>` 
    
2.  调用第三方程序
    
    复制代码`plus.runtime.launchApplication( appInf, errorCB );` 
    
    -   appInf: ( ApplicationInf ) 必选 要启动第三方程序的描述信息
    -   errorCB: ( LaunchErrorCallback ) 必选 启动第三方程序操作失败的回调函数  
        启动第三方程序失败时回调，并返回失败信息。
    
    复制代码 `<template>  
            <view>  
                <button class="button" type="primary" @click="launchApp">打开淘宝</button>  
            </view>  
        </template>` 
    
    复制代码 `<script>  
    export default {  
        data() {  
            return {  
                url: 'https://uniapp.dcloud.io/'  
            };  
        },  
        onLoad(op) {},  
        methods: {  
            launchApp() {  
                let _this = this;  
                
                if (plus.os.name == 'Android') {  
                    plus.runtime.launchApplication(  
                        {  
                            pname: 'com.taobao.taobao'  
                        },  
                        function(e) {  
                            console.log('Open system default browser failed: ' + e.message);  
                        }  
                    );  
                } else if (plus.os.name == 'iOS') {  
                    plus.runtime.launchApplication({ action: 'taobao://' }, function(e) {  
                        console.log('Open system default browser failed: ' + e.message);  
                    });  
                }  
    
            }  
        }  
    };  
    </script>` 
    

## 常用URLscheme

复制代码`[  
    // 只在 ios 中生效  
    {  
        name: 'App Store',  
        scheme: 'itms-apps://'  
    },  
    {  
        name: '支付宝',  
        pname: 'com.eg.android.AlipayGphone',  
        scheme: 'alipay://'  
    },  
    {  
        name: '淘宝',  
        pname: 'com.taobao.taobao',  
        scheme: 'taobao://'  
    },  
    {  
        name: 'QQ',  
        pname: 'com.tencent.mobileqq',  
        scheme: 'mqq://'  
    },  
    {  
        name: '微信',  
        pname: 'com.tencent.mm',  
        scheme: 'weixin://'  
    },  
    {  
        name: '京东',  
        pname: 'com.jingdong.app.mall',  
        scheme: 'openApp.jdMobile://'  
    },  
    {  
        name: '新浪微博',  
        pname: 'com.sina.weibo',  
        scheme: 'sinaweibo://'  
    },  
    {  
        name: '优酷',  
        pname: 'com.youku.phone',  
        scheme: 'youku://'  
    }  
]` 

## 更多实用例子

除了简单的打开App，我们更多的时候想要直达。这里汇总了很多有用的直达案例：

-   使用应用商店打开指定App，可用于引导评分
-   强制使用应用宝打开指定App
-   打开淘宝搜索页面。需要你要做淘宝客，需要向淘宝申请自己的scheme参数并传入。
-   打开地图并指定地点
-   打开qq并到指定聊天界面，可用于客服  
    具体代码见下：

复制代码`<template>  
    <view>  
        <page-head title="通过scheme打开三方app示例"></page-head>  
        <button class="button" @click="openBrowser('https://uniapp.dcloud.io/h5')">使用浏览器打开指定URL</button>  
        <button class="button" @click="openMarket()">使用应用商店打开指定App</button>  
        <button class="button" @click="openMarket('com.tencent.android.qqdownloader')">强制使用应用宝打开指定App</button>  
        <button class="button" @click="openTaobao('taobao://s.taobao.com/search?q=uni-app')">打开淘宝搜索页面</button>  
        <button class="button" @click="openMap()">打开地图并指定地点</button>  
        <view class="uni-divider">  
            <view class="uni-divider__content">打开QQ</view>  
            <view class="uni-divider__line"></view>  
        </view>  
        <view class="uni-padding-wrap">  
            <form @submit="openQQ">  
                <view>  
                    <view class="uni-title">请输入聊天对象QQ号：</view>  
                    <view class="uni-list">  
                        <view class="uni-list-cell">  
                            <input class="uni-input" name="qqNum" type="number"/>  
                        </view>  
                    </view>  
                </view>  
                <view>  
                    <view class="uni-title">请选择QQ号类型：</view>  
                    <radio-group class="uni-flex" name="qqNumType">  
                        <label>  
                            <radio value="wpa" checked=""/>普通QQ号</label>  
                        <label>  
                            <radio value="crm" />营销QQ号(无需加好友直接聊天)</label>  
                    </radio-group>  
                </view>  
                <view class="uni-btn-v uni-common-mt">  
                    <button class="button" formType="submit">打开qq并到指定聊天界面</button>  
                </view>  
            </form>  
        </view>  
    </view>  
</template>  

<script> export default {  
    data() {  
        return {  

        };  
    },  
    methods: {  
        openBrowser(url){  
            plus.runtime.openURL(url)  
        },  
        openMarket(marketPackageName) {  
            var appurl;  
            if (plus.os.name=="Android") {  
                appurl = "market://details?id=io.dcloud.HelloH5"; 
            }  
            else{  
                appurl = "itms-apps://itunes.apple.com/cn/app/hello-uni-app/id1417078253?mt=8";  
            }  
            if (typeof(marketPackageName)=="undefined") {  
                plus.runtime.openURL(appurl, function(res) {  
                    console.log(res);  
                });  
            } else{
                if (plus.os.name=="Android") {  
                    plus.runtime.openURL(appurl, function(res) {  
                        plus.nativeUI.alert("本机没有安装应用宝");  
                    },marketPackageName);  
                } else{  
                    plus.nativeUI.alert("仅Android手机才支持应用宝");  
                }  
            }  
        },  
        openTaobao(url){  
            plus.runtime.openURL(url, function(res) {  
                uni.showModal({  
                    content:"本机未检测到淘宝客户端，是否打开浏览器访问淘宝？",  
                    success:function(res){  
                        if (res.confirm) {  
                            plus.runtime.openURL("https://s.taobao.com/search?q=uni-app")  
                        }  
                    }  
                })  
            });  
        },  
        openMap(){  
            var url = "";  
            if (plus.os.name=="Android") {  
                var hasBaiduMap = plus.runtime.isApplicationExist({pname:'com.baidu.BaiduMap',action:'baidumap://'});  
                var hasAmap = plus.runtime.isApplicationExist({pname:'com.autonavi.minimap',action:'androidamap://'});  
                var urlBaiduMap = "baidumap://map/marker?location=39.968789,116.347247&title=DCloud&src=Hello%20uni-app";  
                var urlAmap = "androidamap://viewMap?sourceApplication=Hello%20uni-app&poiname=DCloud&lat=39.9631018208&lon=116.3406135236&dev=0"  
                if (hasAmap && hasBaiduMap) {  
                    plus.nativeUI.actionSheet({title:"选择地图应用",cancel:"取消",buttons:[{title:"百度地图"},{title:"高德地图"}]}, function(e){  
                        switch (e.index){  
                            case 1:  
                                plus.runtime.openURL(urlBaiduMap);  
                                break;  
                            case 2:  
                                plus.runtime.openURL(urlAmap);  
                                break;  
                        }  
                    })  
                }  
                else if (hasAmap) {  
                    plus.runtime.openURL(urlAmap);  
                }  
                else if (hasBaiduMap) {  
                    plus.runtime.openURL(urlBaiduMap);  
                }  
                else{  
                    url = "geo:39.96310,116.340698?q=%e6%95%b0%e5%ad%97%e5%a4%a9%e5%a0%82";  
                    plus.runtime.openURL(url); 
                }  
            } else{  
                
                plus.nativeUI.actionSheet({title:"选择地图应用",cancel:"取消",buttons:[{title:"Apple地图"},{title:"百度地图"},{title:"高德地图"}]}, function(e){  
                    console.log("e.index: " + e.index);  
                    switch (e.index){  
                        case 1:  
                            url = "http://maps.apple.com/?q=%e6%95%b0%e5%ad%97%e5%a4%a9%e5%a0%82&ll=39.96310,116.340698&spn=0.008766,0.019441";  
                            break;  
                        case 2:  
                            url = "baidumap://map/marker?location=39.968789,116.347247&title=DCloud&src=Hello%20uni-app";  
                            break;  
                        case 3:  
                            url = "iosamap://viewMap?sourceApplication=Hello%20uni-app&poiname=DCloud&lat=39.9631018208&lon=116.3406135236&dev=0";  
                            break;  
                        default:  
                            break;  
                    }  
                    if (url!="") {  
                        plus.runtime.openURL( url, function( e ) {  
                            plus.nativeUI.alert("本机未安装指定的地图应用");  
                        });  
                    }  
                })  
            }  
        },  
        openQQ: function (e) {  
            
            
            plus.runtime.openURL('mqqwpa://im/chat?chat_type=' + e.detail.value.qqNumType + '&uin=' + e.detail.value.qqNum,function (res) {  
                plus.nativeUI.alert("本机没有安装QQ，无法启动");  
            });  
        }  
    }  
}; </script>  
<style> .button {  
    margin: 30upx;  
    color: #007AFF;  
} </style>` 

## 给自己的App设置scheme

可在manifest中可配置。  
[Android配置方法](https://ask.dcloud.net.cn/article/409)  
[iOS配置方法](http://ask.dcloud.net.cn/article/64)
