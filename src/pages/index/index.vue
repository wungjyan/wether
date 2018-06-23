<template>
  <div class="container">
    <div class="top">
      <div class="header">
        <span @click="showSearch">其他城市</span>
        <span>更新时间：{{loc}}</span>
      </div>
      <div class="wether">
        <p class="tmp"><span>{{tmp}}°</span> <span>{{location}}</span></p>
        <p class="txt">{{condTxt}}</p>
        <p class="wind">{{windDir}} {{windSc}}级</p>
      </div>
      <div class="comf">
        舒适度：{{comf}}
      </div>
    </div>
    <div class="tip">
      <span style="color:#E03636;margin-right:10px;">穿衣贴士</span>{{drsg}}
    </div>
    <div class="more">
      <div>
        <p class="time">今天</p>
        <div class="line"></div>
        <p class="tmp">白天：{{oneD}}</p>
        <p class="tmp">夜晚：{{oneN}}</p>
        <p class="tmp_max">高温：{{oneMax}}℃</p>
        <p class="tmp_min">低温：{{oneMin}}℃</p>
      </div>
      <div>
        <p class="time">明天</p>
        <div class="line"></div>
        <p class="tmp">白天：{{twoD}}</p>
        <p class="tmp">夜晚：{{twoN}}</p>
        <p class="tmp_max">高温：{{twoMax}}℃</p>
        <p class="tmp_min">低温：{{twoMin}}℃</p>
      </div>
      <div>
        <p class="time">后天</p>
        <div class="line"></div>
        <p class="tmp">白天：{{threeD}}</p>
        <p class="tmp">夜晚：{{threeN}}</p>
        <p class="tmp_max">高温：{{threeMax}}℃</p>
        <p class="tmp_min">低温：{{threeMin}}℃</p>
      </div>
    </div>
    <div class="warning" v-if="showWarning">由于获取地理位置失败，您当前显示的是默认地区的天气，您可自行搜索其他城市</div>
    <div class="search" :style="{height: searchH}">
      <input type="text" v-model="value" placeholder="此处输入城市，如：北京"/>
      <button size='mini' @click="searchConf">确定</button>
      <p class="warn">搜索提示：</p>
      <p>直接搜市名，如：北京或北京市</p>
      <p>可以搜拼音，如：beijing</p>
      <p>可以带上区名，如：朝阳,北京（中间是英文逗号）</p>
      <div class="close" @click="closeSearch">
        <icon type="cancel" size="40" color="#5e8579"/>
      </div>
    </div>
  </div>
 
</template>

<script>
export default {
  data() {
    return {
      tmp:'', //当前实时温度
      location:'', //当前地区
      condTxt:'', //当前天气
      windDir:'',//当前风向
      windSc:'' ,//当前风力
      locTime:'',//当地更新时间

      comf:'', //舒适度
      drsg:'', //穿衣建议

      oneD:'', //第一天白天气候
      oneN:'', //第一天晚上气候
      oneMax:'', //第一天最高温
      oneMin:'',  //第一天最低温

      twoD:'', //第二天白天气候
      twoN:'', //第二天晚上气候
      twoMax:'', //第二天最高温
      twoMin:'',  //第二天最低温

      threeD:'', //第三天白天气候
      threeN:'', //第三天晚上气候
      threeMax:'', //第三天最高温
      threeMin:'',  //第三天最低温

      searchH:0,
      value:'',
      showWarning:false
    };
  },
  computed: {
    loc(){
      return this.locTime.split(' ')[1]
    }
  },
  components: {},
  methods: {
    //天气赋值函数
    getValue(that,data){
      that.tmp = data.now.tmp
      that.location = data.basic.location
      that.condTxt = data.now.cond_txt
      that.windDir = data.now.wind_dir
      that.windSc = data.now.wind_sc
      that.locTime = data.update.loc
    },

    //当没获取本地信息时，获取默认城市的天气
    getDefaultWether(key,that){
      wx.request({
        url:'https://free-api.heweather.com/s6/weather/now',
        data:{
          location:'朝阳,北京',
          key
        },
        success:function(res){
          console.log('默认获取：',res)
          let data = res.data.HeWeather6[0]
          that.getValue(that,data)
        }
      })
    },

    //获取实况天气
    getNowWether(key,location,that) {
      wx.request({
        url: "https://free-api.heweather.com/s6/weather/now",
        data: {
          location,
          key
        },
        success: function(res) {
          console.log(res);
          let data = res.data.HeWeather6[0]
          that.getValue(that,data)
        }
      });
    },

    //当没获取本地信息时，获取默认城市的生活信息
    getDefaultLiftStyle(key,that){
      wx.request({
        url:'https://free-api.heweather.com/s6/weather/lifestyle',
        data:{
          location:'朝阳,北京',
          key
        },
        success:function(res){
         console.log(res)
         let data = res.data.HeWeather6[0].lifestyle
         that.comf = data[0].txt
         that.drsg = data[1].txt
        }
      })
    },


    //获取当前生活信息
    getLifeStyle(key,location,that){
      wx.request({
        url:'https://free-api.heweather.com/s6/weather/lifestyle',
        data:{
          location,
          key
        },
        success:function(res){
         console.log(res)
         let data = res.data.HeWeather6[0].lifestyle
         that.comf = data[0].txt
         that.drsg = data[1].txt
        }
      })
    },

    //近三天气温赋值函数
    getThreeValue(that,data){
      that.oneD = data[0].cond_txt_d
      that.oneN = data[0].cond_txt_n
      that.oneMax = data[0].tmp_max
      that.oneMin = data[0].tmp_min

      that.twoD = data[1].cond_txt_d
      that.twoN = data[1].cond_txt_n
      that.twoMax = data[1].tmp_max
      that.twoMin = data[1].tmp_min

      that.threeD = data[2].cond_txt_d
      that.threeN = data[2].cond_txt_n
      that.threeMax = data[2].tmp_max
      that.threeMin = data[2].tmp_min
    },

    //当没获取本地信息时，获取默认城市的三天天气
    getDefaultThreeWether(key,that){
      wx.request({
        url:'https://free-api.heweather.com/s6/weather/forecast',
        data:{
          location:'朝阳,北京',
          key
        },
        success:function(res){
          console.log(res)
          let data = res.data.HeWeather6[0].daily_forecast
          that.getThreeValue(that,data)
        }
      })
    },
    //获取近三天的气温信息
    getThreeWether(key,location,that){
      wx.request({
        url:'https://free-api.heweather.com/s6/weather/forecast',
        data:{
          location,
          key
        },
        success:function(res){
          console.log(res)
          let data = res.data.HeWeather6[0].daily_forecast
          that.getThreeValue(that,data)
        }
      })
    },

    //显示搜索
    showSearch(){
      this.searchH = '70%'
    },
    
    // 点击关闭搜索框
    closeSearch(){
     this.searchH = 0
    },
    //确定搜索
    searchConf(){
      let location = this.value
      if(!location){
        this.searchH = 0
        return;
      }
      let key = 'e96b77187156424095f4a6d521582e7d'
      this.getNowWether(key,location,this)  //获取当前地区气温信息
      this.getLifeStyle(key,location,this)  //获取当前地区生活信息
      this.getThreeWether(key,location,this) //获取当前地区三天气温
      this.searchH = 0
      this.showWarning = false
    }
  },
  onShow() {
    console.log('进来index:',wx.getStorageSync('location'))
    //天气api的key
    let key = "e96b77187156424095f4a6d521582e7d";
   
    let location;
    if (wx.getStorageSync("location")) {
      location = wx.getStorageSync("location");
      console.log('获取',location);
      location = location.longitude + "," + location.latitude;
      this.getNowWether(key,location,this)  //获取当前地区气温信息
      this.getLifeStyle(key,location,this)  //获取当前地区生活信息
      this.getThreeWether(key,location,this) //获取当前地区三天气温
    } else {
      this.getDefaultWether(key,this)  //获取默认城市气温信息
      this.getDefaultLiftStyle(key,this) //获取默认城市生活信息
      this.getDefaultThreeWether(key,this) //获取默认城市三天天气情况
      this.showWarning = true
    }
  }
};
</script>

<style scoped>
.top {
  background: #56a36c;
  margin: 10px;
  padding: 5px;
  color: #fff;
  box-shadow: 5px 5px 10px #5e8579;
}
.top .header {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
}
.top .wether {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 15px;
  font-size: 15px;
}
.wether .tmp span:first-child {
  color: #fff;
  font-size: 25px;
  margin-right: 10px;
}
.wether .tmp span:last-child {
  color: #e03636;
}
.top .comf {
  margin-top: 30px;
  font-size: 15px;
}
.tip {
  color: #5e8579;
  font-size: 14px;
  line-height: 18px;
  padding: 5px;
  margin: 10px;
}
.more {
  display: flex;
  justify-content: space-around;
  color: #5e8579;
  font-size: 15px;
  margin-top: 30px;
}
.more > div {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 30%;
  border: 1px solid #5e8579;
  border-radius: 5px;
  padding-bottom: 5px;
}
.more .time {
  margin: 5px 0 5px;
}
.more .line {
  width: 80%;
  height: 1px;
  background: #e03636;
  border: none;
}
.more .tmp {
  margin: 5px 0 5px;
}

.warning{
  width: 100%;
  padding:5px;
  margin-top: 10px;
  font-size: 14px;
  color: #e03636;
}

.search{
  position: fixed;
  bottom: 0px;
  z-index: 3;
  width: 100%;
  background: #fff;
  color: #5e8579;
  padding:5px;
  transition: height 0.5s;
}
.search p{
  font-size: 14px;
}
.search input{
  margin:5px 0 10px 0;
}
.search .close{
  width: 40px;
  margin:40% auto;
}
</style>
