<template>
<div></div>
</template>

<script type="text/ecmascript-6">
export default {
  data() {
    return {
    }
  },
  methods: {
      //获取地理位置并保存到本地
    getLocation(){
        wx.getLocation({
          success:function(res){
            var longitude = res.longitude;
            var latitude = res.latitude;
            wx.setStorageSync('location',{longitude,latitude})
            wx.redirectTo({
                url:'/pages/index/main'
            })
          },
          fail:function(err){
            wx.redirectTo({
                url:'/pages/index/main'
            })
          }
        })
    }  
  },
  onShow(){
      const location = wx.getStorageSync('location')
      if(!location){
        this.getLocation()
      }else{
        wx.redirectTo({
          url:'/pages/index/main'
        })
      }
      
      
  }

}
</script>

<style>
</style>

