<template>
  <div class="hello">
    <keep-alive>
      <slider :autoPlay="true" :loop="true" :duringTime="2000">
        <div v-for="imgUrl in imgList" class="img-item">
          <img :src="imgUrl">
        </div>
      </slider>
    </keep-alive>
  </div>
</template>

<script>
import Slider from 'common/slider/slider'
export default {
  name: 'HelloWorld',
  data(){
    return {
      imgList:[]
    }
  },
  created(){
    var img1 = require('common/img/img1.jpg')
    var img2 = require('common/img/img2.jpg')
    var img4 = require('common/img/img4.jpg')
    var img5 = require('common/img/img5.jpg')
    this.imgList.push(img1,img2,img4,img5)
  },
  components:{
    Slider
  }
}
</script>

