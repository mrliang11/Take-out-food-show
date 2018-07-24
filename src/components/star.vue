<template>
  <div id="star">
    <span class="star-img" v-for="(newClass,index) in newClasses" :class = "newClass" :key="index"></span>
  </div>
</template>

<script>
const LENGTH = 5
let ON = "on"
let HALF = "half"
let OFF = "off"
export default {
  props : {
    score : {
      type : Number
    }
  },
  computed : {
    newClasses(){
      let totalClass = []
      let score = Math.floor(this.score * 2)/2
      let allStar = Math.floor(this.score)
      let halfStar = score - allStar
      for(let i = 0 ; i < allStar ; i++){
        totalClass.push(ON)
      }
      if(halfStar != 0){
        totalClass.push(HALF)
      }
      while(totalClass.length < LENGTH){
        totalClass.push(OFF)
      }
      return totalClass
    }
  }
}
</script>
<style lang="stylus" ref="stylesheet/stylus">
@import '../assets/stylus/index.styl';
  #star
    height 24px
    display inline-block
    .star-img
      display inline-block
      width 24px
      height 24px
      background-size 24px 24px
      margin-left 24px
    .on
      background-image url("../assets/images/star24_on@2x.png")
    .half
      background-image url("../assets/images/star24_half@2x.png")
    .off
      background-image url("../assets/images/star24_off@2x.png")
</style>
