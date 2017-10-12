<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  data(){
    return {
        currentPageIndex: 0
    }
  },
  props:{
    loop:{
      type: Boolean,
      default: true
    },
    autoPlay:{
      type: Boolean,
      default: true  
    },
    duringTime:{
      type: Number,
      default: 2000    
    }
  },
  mounted(){
    this._setSliderWidth()
    this._initSlider()
    if (this.autoPlay) {
      this._play()
    }
  },
  methods:{
    _setSliderWidth(){
      console.log(this.$refs.sliderGroup)
      this.sliderItems = this.$refs.sliderGroup.children
      if(this.sliderItems.length==0){
        return
      }
      let width = 0
      let sliderWidth = this.$refs.slider.clientWidth
      for(let i = 0; i<this.sliderItems.length; i++){
        let child = this.sliderItems[i]
        child.style.width = sliderWidth + 'px'
        console.log(child)
        width += sliderWidth
      }
      if(this.loop){
          width += 2 * sliderWidth
      }
      console.log(width)
      this.$refs.sliderGroup.style.width = width + 'px'
    },
    _initSlider(){
      this.slider = new BScroll(this.$refs.slider,{
          scrollX: true,
          scrollY: false,
          momentum: false,
          snap: {
              loop: this.loop,
              threshold: .3,
              speed: 400
          }
      })
      this.slider.on('scrollEnd',()=>{
        let pageIndex = this.slider.getCurrentPage().pageX
        this.currentPageIndex = pageIndex
        if (this.loop) {
          pageIndex -= 1
        }
        if(this.autoPlay){
          clearTimeout(this.timer)
          this._play()
        }
      })
    },
    _play(){
      let pageIndex = this.currentPageIndex + 1
      if (this.loop) {
        pageIndex += 1
      }
      this.timer = setTimeout(() => {
        this.slider.goToPage(pageIndex, 0, 400)
      }, this.duringTime)
    }
  }
}
</script>

<style lang="stylus" scoped>
.slider
  width 100%
  overflow hidden
  .img-item
    float left
    img
      width 100%
      height auto
</style>
