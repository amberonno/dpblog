title: dpapp-apollo迁移指南
date: 2015-12-02 13:57:15
tags:
 - 工作
---

去EFTE指南
=============
EFTE和DPAPP对比表格
=============

| efte方法                            | 对应DPAPP方法                                            | 说明  |
| ---------------------------------- |:------:| -----:|
| send_message                       | 无    |  |
| ajax                               | ajax  |    |
| action.reloadPage                  | 无    | 使用location.reload()代替   |
| action.get                         | 无    |    |
| action.open                        | openScheme、jumpToScheme     |  直接替换（jumpToScheme会关闭之前的页面）  |
| action.openUrl                     | openScheme、jumpToScheme    |    |
| action.willBack                    | 无    |    |
| action.back                        | 无     |    |
| aciton.dismiss                     | closeWindow   |  直接替换  |
| getEnv                             | getDeviceInfo   | 直接替换   |
| getAppVersion                      | getVersion    |  直接替换  |
| getPlatform                        | getDeviceInfo    |直接替换（deviceInfo包含platform信息）    |
| enableRefresh                      | 无     |    |
| startRefresh                       | setPullDown    |  直接替换，不需要调用enableRefresh  |
| stopRefresh                        | stopPullDown     | 直接替换   |
| isSupportEIM                       | isSupportEIM    |  直接替换  |
| openEIM                            | openEIM     | 直接替换   |
| genUUID                            | getDeviceInfo     | 直接替换（deviceInfo包含uuid信息）   |
| setTitle                           | setTitle   |直接替换    |
| setTitleItems                      | 无      |    |
| takePhoto                          | uploadImage     | 直接替换（uploadImage支持从相册选择并且拍照上传）   |
| takePhotoByName                    | 无     |    |
| log                                | 无     |    |
| setBarButtons                      | setLLButton, setLRButton, setRLButton, setRRButton    | 依据要设置的按钮位置直接替换   |
| publish                            | publish   | 直接替换   |
| subscribe                          | subscribe    |  直接替换  |
| unsubscribe                        | unsubscribe    | 直接替换   |
| multilevelFromResource             | 无      |    |
| multilevel                         | 无    |    |
| datePicker                         | 无     | 废弃   |
| date                               | date   | 直接替换   |
| logout                             | logout     | 直接替换   |
| geo.getCurrentPosition             | getLocation    | 直接替换   |
| geo.getWifiInfo                    | getWifiInfo    | 直接替换   |
| showPhoto                          | showPhoto    | 直接替换   |
| cache.save                         | cacheSave    |直接替换    |
| cache.load                         | cacheLoad    | 直接替换   |
| cache.remove                       | cacheDelete    | 直接替换   |
| cache.removeAll                    | cacheClear    | 直接替换   |
| push.getInfo                       | 无    |    |
| push.getExtra                      | 无     |    |
| ga.sendEvent                       | 无     |  废弃  |
| ga.sendView                        | 无     | 废弃   |
| ga.sendTiming                      | 无     |  废弃  |
| ga.sendException                   | 无     |  废弃  |
| ga.setCustomDimension              | 无      | 废弃   |
| ga.setCustomMetric                 | 无   |  废弃  |



patch-app.js
=======

The foundation js bridge for efte projects, it provides js bridges that used in webview or browser (when you do local development).

Install
=======

Use cortex CLI tool to install dpapp-apollo.

```bash
$ cortex install dpapp-apollo --save
```

Require `dpapp-apollo` in the js you will use it.

```js
var DPApp = require("dpapp-apollo");
```

DPAPP的调用协议
========
所有接口若非特别说明，所有接口**只接受一个javascript对象 只接受一个javascript对象 只接受一个javascript对象**(重要的事情说三遍)作为参数，其中success，fail分别为成功与失败后的回调。 同时对于延时反馈的场景（如监听广播，按钮被点击的回调等），可以传入handle回调函数来处理。 回调函数接受一个json对象作为参数。

EFTE接口变化
========
## - send_message
废弃

## - publish
Use to pushlish message to other page. Always use with `subscribe`.

**EFTE版本**  `Efte.publish(name, value)`

**DPAPP版本**  `DPApp.publish({action:'',data:{},success:function(){}})`


- action: `String`, must a uniq string identifier in an App, eg: `'myMessageName'`
- data: `Object`, JSON object, Android limit size 30KB.


```js
            DPApp.publish({
                action: 'myMessageName',
                data: {
                    'info': 'detail'
                },
                success: function(){
                    alert("发布成功");
                }
            });
```

## - subscribe

**subscribe**
Use to receive message from `publish`.



**EFTE版本**  `Efte.subscribe(name, callback)`

**DPAPP版本**  `DPApp.subscribe({action:'',handle:function(){},success:function(){}})`

- action: `String`, must a uniq string identifier in an App, eg: `'unit-m-customer-add-customer'`
- success: `Function`, call when subscribe success.
- handle: `Function`, call with received JSON object message.

```js
            DPApp.subscribe({
                action: 'myMessageName',
                success: function(){
                    alert("订阅成功");
                },
                handle: function(e){
                    alert(JSON.stringify(e))
                }
            });
```

## - setTitle

Set page title, it will show on header.


**EFTE版本**  `Efte.setTitle(title)`

**DPAPP版本**  `DPApp.setTitle({title:'',success:function(){}})`

- title: `String`

```js
            DPApp.setTitle({
                title:'I am a title',
                success: function(e){
                    console.debug(e);
                }
            });```

## - action.open 废弃，替换成openScheme和jumpToScheme（会关闭当前窗口）

**废弃，统一用openScheme和jumpToScheme**

## - openScheme和jumpToScheme（会关闭当前窗口）

打开新的webview，extra参数会用于拼url，并做对value做encode

```js
              DPApp.openScheme({
                  url: "dpcrm://web",
                  extra: {
                      url:'http://www.dianping.com'
                  },
                  success: function(){
                      alert('跳转成功');
                      // 跳转成功
                  }
              });
```
```js
            DPApp.jumpToScheme({
                url: "dpcrm://web",
                extra: {
                    url:'http://www.dianping.com'
                },
                success: function(){
                    alert('跳转成功');
                    // 跳转成功
                }
            });
```


## - action.back

**废弃（openScheme和jumpToScheme打开的新页面会自带返回按钮）** `Efte.action.back(animate)`
`

## - action.dismiss 替换成**closeWindow**

Close current modal webview.

**EFTE版本**  `Efte.action.dismiss()`

**DPAPP版本**  `DPApp.closeWindow()`

```js
            DPApp.closeWindow();
```

## - action.openUrl

**废弃，统一用openScheme和jumpToScheme** `Efte.action.openUrl(url, modal, animate)`

## - action.get

**废弃** `Efte.action.get(callback)`

## - action.reloadPage

**废弃** `Efte.action.reloadPage()请替换成js原生location.reload()`
Reload current page.

## - getAppVersion替换成**getVersion**

Get App version.
**EFTE版本** `Efte.getAppVersion(callback)`
**DPAPP版本** `DPAPP.getVersion({success:function(){}})`


- success: `Function`, called with version string.

```js
            DPApp.getVersion({
                success: function(e){
                    console.debug(e);
                    alert('version: '+e.version);
                }
            });
```

Plugins
=======

## - ajax

Request by native, no problem with CORS.
**EFTE版本** `Efte.ajax(options)  (options中的error回调替换成fail)`
**DPAPP版本** `DPAPP.ajax(options)`

**options** `Object`

- method: `String`, eg: `'GET | POST | PUT | DELETE'`.
- url: `String`, eg: `'http://efte.io/api/test'`.
- data: `Object`, JSON object.
- success: `Function`, called with server result.
- fail: `Function`, called with `status` and `errMsg`.

```js
            DPApp.ajax({
                method:'post',
                url:'https://a.dper.com/demo/demo',
                data:{
                    name:'app'
                },
                success:function(result){
                    alert(JSON.stringify(result));
                },
                fail:function(result){
                    alert(JSON.stringify(result));
                }
            });
```

## - setBarButtons **（EFTE版本的接口，已经弃用）**

废弃，替换成'setLLButton', 'setLRButton', 'setRLButton', 'setRRButton'四个方法，用来设置bar的四个位置的按钮
**EFTE版本** `Efte.setBarButtons(buttons)`

- buttons: `Array`, `[button...]`

**Button** `Object`

- button: `Object`
- title: `String`
- action: `Function`, handler for user taped the button.

```js
Efte.setBarButtons([{
  title: '设置',
  action: function () {
    console.log('taped');
  }
}]);
```

## - setLLButton

'setLLButton', 'setLRButton', 'setRLButton','setRRButton'四个方法参数一样
**DPAPP版本** ` DPApp.setLLButton(BtnConfig)`


**BtnConfig** `Object`

- text: `按钮文字`
- icon: `String` 文字按钮或者图片按钮。 icon属性定义了本地资源的名称，目前仅支持H5_Search（搜索）、H5_Back（返回）、H5_Custom_Back（返回图标+文字）、H5_Share（分享） icon属性会覆盖text属性
- success: `Function`, callback when set successfully.
- handle: `Function`, handler for user taped the button.
```js
            DPApp.setLLButton({
                icon: "H5_Search",
                success: function(){
                    alert("设置成功");
                },
                handle: function(){
                    alert("ll按钮被点击");
                }
            });
```

```js
            DPApp.setLRButton({
                text: "lr按钮",
                success: function(){
                    alert("设置成功");
                },
                handle: function(){
                    alert("lr按钮被点击");
                }
            });
```

## - date

Useful Datetime Picker.

**EFTE版本的date** `Efte.date(options, callback)`
**DPAPP版本的date** `DPApp.date(options)`

**DPAPP版本的options保持一致，和EFTE唯一的区别在于把callback作为options对象的success属性**

- type: `String`, set Datetime Picker type. eg: `'date | time | datetime(default)'`
- default: `String`, set default show datetime in Datetime Picker. eg: `'2014-11-11 | 12:12:12 | 2014-11-11 12:12:12(default)'`
- minuteInterval: `Integer`, must divide exactly by 60, set the minute step of Datetime Picker.
- minDate: `Integer`, set the min indate, milliseconds start at 1970. eg: `1402012800000`
- maxDate: `Integer`, set the max indate, milliseconds start at 1970. eg: `1414569430214`

```js
            DPApp.date({
                type:'datetime',
                default:'2015-11-11 12:12:12',
                maxDate:1448432440113,
                minDate:1446307200000,
                minuteInterval:5,
                success:function(value){
                    alert(JSON.stringify(value));
                    console.debug('jsb-date',value);
                }
            });
```

## - geo.getCurrentPosition替换成getLocation

**EFTE版本getCurrentPosition** `Efte.geo.getCurrentPosition(success, error)`
**DPAPP版本getLocation** `DPApp.getLocation({success:function(){}})`

coords like `{ lng: 121.4268740794 , lat: 31.2202555095 }`.

```js
            DPApp.getLocation({
                success: function(result){
                    alert('lat: '+result.lat+' , lng: '+result.lng);
                }
            });
```

## - takePhoto替换成**uploadImage**

**EFTE版本takePhoto** `Efte.takePhoto(callback)`
**DPAPP版本uploadImage** `DPApp.uploadImage({})`
Pick and upload photo from photo gallary or camera;

- callbck: `Function`, called with `photoInfo`.
- maxNum: 允许上传的最大张数.
- uploadUrl: 上传地址.

```js
            DPApp.uploadImage({
                maxNum:leftNum,
                uploadUrl: self.options.uploadUrl,
                callback:function(data){
                    if(data.progress=='uploading'){
                        var result=(data.result?data.result:{});
                        if(result.url){
                            var image=result;
                            self.show(image);
                        }
                    }
                }
            });
```

## - takePhotoByName

废弃

## - showPhoto

Show large photo.
**EFTE版本showPhoto** `Efte.showPhoto(photo, onDelete)`
**DPAPP版本showPhoto** `DPApp.showPhoto({url:'',editable:true,success:function(){}})`


- url: `String`, full size image url.
- editable: `Boolean`, 是否可以删除图片
- success: `Function`, 删除成功的回调函数

```js
            DPApp.showPhoto({
                url: "http://i1.s1.dpfile.com/pc/399fdac2b46a80ef85315a906504a3d0(700x700)/thumb.jpg",
                editable:true,
                success: function(result){
                    alert(JSON.stringify(result));
                }
            });
```

## - enableRefresh

**废弃** `Efte.enableRefresh()`

## - startRefresh替换成**setPullDown**
it will be called when user pull down the webview

**EFTE版本startRefresh** `Efte.startRefresh = function () {}`
**DPAPP版本setPullDown** `DPApp.setPullDown({handle:function(){}})`
- handle: `Function`, 下拉刷新后的回调函数


```js

            DPApp.setPullDown({
                handle: function(){
                    alert(1);
                    DPApp.ajax({
                        url:'',
                        success: function(){
                            DPApp.stopPullDown({

                            });
                        },
                        fail:function(){
                            DPApp.stopPullDown({

                            });
                        }
                    });
                }
            });
```

## - stopRefresh替换成stopPullDown
Use to stop refresh at the end of `startRefresh`.

**EFTE版本stopRefresh** `Efte.stopRefresh()`
**DPAPP版本stopPullDown** `DPApp.stopPullDown({})`

```js
                            DPApp.stopPullDown({

                            });
```

## - multilevelFromResource

**废弃**
## - multilevel

**废弃**