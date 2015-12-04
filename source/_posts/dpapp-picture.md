title: '{dpapp-picture}'
date: 2015-12-03 15:33:29
tags:
---



**Picture构造函数的接收参数** `Object`

- title: `String`,添加按钮的文字
- maxNumber:`Integer`,最大上传个数
- uploadUrl: `String`, 上传地址，必传参数.
- fail: `Function`, 上传失败的回调.
- editable: `Boolean`, 是否允许用户删除上传过的图片，默认为true.
- complete: `Function`, 选择的图片全部上传完成后的回调

**Picture对象的常用方法**

- getImages: 获取上传成功的图片数组
- getImage(index):获取图片数组中的某张图片
- remove(index): 删除图片数组中的某张图片
- empty: 清空图片数组

与EFTE的对比
=============
dpapp版本的图片上传在native实现，efte的上传逻辑用js实现，picture构造函数的输入参数以此文档为主，picture对象的常用方法和efte一致（废弃了上传的相关逻辑）

渲染出来的html片段
=============
```html
<div class="image-icon" data-id="image-item" data-index="1">
    <img src=""/>
</div>
<div class="image-icon add" data-bind="add-image-btn">
    <span>添加图片</span>
</div>
```


如果想自己更改图片和按钮样式，请参考上面html片段的class属性用css进行渲染
=============
```css
        #imagePickerContainer::after{
            content: '';
            clear: both;
            display: table;
        }
        .image-icon{
            width: 22%;
            margin-right: 3%;
            box-sizing: border-box;
            float: left;
        }
        .image-icon img{
            width: 100%;
        }
```
使用示例
=============
```js
            var Picture = require('dpapp-picture');
            var picture=new Picture('imagePickerContainer',{
                title:'添加图片',
                maxNumber:5,
                uploadUrl: "https://apollo.51ping.com/app-visit/attach/upload",
                fail:function(data){
                    alert(JSON.stringify(data));
                },
                complete:function(data){
                    alert(JSON.stringify(picture.getImages()));
                    alert(JSON.stringify(data));
                }
            });
```
