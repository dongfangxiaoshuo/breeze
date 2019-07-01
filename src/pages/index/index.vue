<template>
  <div>
    <div><van-button type="default" @click="toLti">识图分类</van-button></div>
    <div><van-button type="default">搜索分类</van-button></div>
    <div> <van-button type="default">分类图</van-button></div>
  </div>
</template>

<script>


export default {
  data () {
    return {
     
    }
  },



  methods: {
    toLti(){
      var that = this;
      wx.chooseImage({
        count: 1,
        sizeType: ['original', 'compressed'],
        sourceType: ['album', 'camera'],
        success (res) {
          // tempFilePath可以作为img标签的src属性显示图片
          //图片转base64
          wx.getFileSystemManager().readFile({
            filePath: res.tempFilePaths[0], //选择图片返回的相对路径
            encoding: 'base64', //编码格式
            success: res => { //成功的回调
              var imgUrl = decodeURIComponent(res.data);//Urlencode解码
              //请求token
              wx.request({
                url: 'https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=XiCStopz8d5hpz4xgYGCf4ul&client_secret=gqptcwnvViQ5FCLhGeo0V78xDap5MaW0', 
                success (res) {
                  var access_token = res.data.access_token;
                  //发送智能识图
                   wx.request({
                    url: 'https://aip.baidubce.com/rest/2.0/image-classify/v2/advanced_general',
                    method:'POST',
                    data:{
                      access_token:access_token,
                      image:imgUrl
                    },
                    header: {
                      "Content-Type": "application/x-www-form-urlencoded"
                    },
                    success (res) {
                      console.log(res.data)
                    }
                  })
                }
              })
              
            }
          })

         
         
   



           
           
        },
        

      })
    },
    
  },

  created () {
   
  }
}
</script>

<style scoped lang=scss>
  div{
    padding: 3rem 0;
    div{  
      padding: .3rem 2.5rem;
      text-align: center;
    }
  }
</style>
