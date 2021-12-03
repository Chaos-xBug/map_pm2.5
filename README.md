# 基于高德地图API可视化PM2.5指数

> - 帮憨憨室友做创新实践作业
> - 用JavaScript不一定要学JavaScript系列
> - 文末有完整源码和数据集的链接

## 获取高德地图API

请自行百度

## 编写head

```javascript
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>2018年12月1日-PM2.5情况</title>
    <style>
        html,
        body,
        #container {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
        }
    </style>
</head>
```

## 编写body

### 调用API

```javascript
<script src="//a.amap.com/Loca/static/dist/jquery.min.js"></script>
<script src="https://webapi.amap.com/maps?v=1.4.15&key=你的key"></script>
<script src="//webapi.amap.com/loca?v=1.3.2&key=你的key"></script>
```

### 导入csv数据集

```javascript
$.get('china5a2.csv', function (data) 
```

### 设置地图参数

```javascript
var map = new AMap.Map('container', {
                mapStyle: 'amap://styles/db9efe6a1745ac24b7269b862f359536',
                zoom: 4,
                center: [107.4976, 32.1697],
                features: ['bg', 'road'],
                // Loca 自 1.2.0 起 viewMode 模式默认为 3D，如需 2D 模式，请显示配置。
                // viewMode: '3D'
            }
```

### 给定经纬度和PM2.5指数

```javascript
layer.setData(data, {
                // lnglat: '经纬度',
                // lnglat: ['lon', 'lat'],
                // 或者使用回调函数构造经纬度坐标
                
                 lnglat: function (obj) {
                     var value = obj.value;
                     var lnglat = [value['经度'], value['纬度']];
                     return lnglat;
                 },
                 
                // 指定数据类型
                type: 'csv'
            });

            layer.setOptions({
                style: {
                    // 圆形半径，单位像素
                    radius: function (obj) {
                     var value = obj.value;
                     var lnglat = value['pm25'] * 0.1;
                     return lnglat*0.2;
                 },
                    // 填充颜色
                    // color: '#5DFBF9',
                    color: 'red',
                    // 描边颜色
                    borderColor: 'red',
                    // 描边宽度，单位像素
                    borderWidth: 1,
                    // 透明度 [0-1]
                    opacity: 1,
                }
            });
```

## 效果图

使用鼠标滚轮可以放大缩小，放大后细节很多，但由于数据集有些大，看起来会与点卡

![image-20211203131134850](https://cdn.jsdelivr.net/gh/Chaos-xBug/img/blog/202112031311541.png)
