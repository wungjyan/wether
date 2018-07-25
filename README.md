# 简单点天气小程序
> 基于mpvue框架的天气小程序

## 图片演示
![](/static/img/show.gif)

## 扫码
![](/static/img/code.jpg)

## 功能
该程序一共有两个页面，分别是`pages/index/`和`pages/location/`。打开小程序后首先进入location页面，功能是询问用户是否可以获取地理位置，如果允许获取，会将获得的地理位置本地存储，并跳转到index页面显示当地天气信息，如果不允许获取用户地理位置，则index页面展示默认城市的天气，并在下方提示用户可以自行搜索其他城市天气。（主要功能代码都集中在pages/index/index.vue 中了，可以直接看此处代码，代码较多，大部分都写上了注释）

该程序具有搜索其他城市功能，搜索模式主要有三种：
- 直接搜城市名，如：北京或北京市
- 搜索城市拼音，如：beijing
- 带上城市的区名，如：朝阳,北京（中间是英文逗号）

以上三种都可以搜索到城市天气信息。

程序展示的信息主要有：
- 实时天气最近更新时间
- 实时天气
- 舒适度提示
- 穿衣提示
- 近三天天气信息

## api
该程序使用和风天气api，主要用到的有：
- 实时天气：https://free-api.heweather.com/s6/weather/now
- 生活指数：https://free-api.heweather.com/s6/weather/lifestyle
- 3-10天天气预报：https://free-api.heweather.com/s6/weather/forecast

api每天可以免费请求1000次。

[api文档参考](http://www.heweather.com/documents/api/s6)




