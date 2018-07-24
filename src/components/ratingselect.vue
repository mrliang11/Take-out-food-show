<template>
  <div id="ratingselect">
    <div class="rating-type border-1px">
      <span @click="select(2,$event)" :class="{'active' : selectType === 2}" class="all">全部</span>
      <span @click="select(0,$event)" :class="{'active' : selectType === 0}" class="some">推荐</span>
      <span @click="select(1,$event)" :class="{'active' : selectType === 1}" class="bad">吐槽</span>
    </div>
    <div @click="toggleContent" class="switch" :class="{'on' : onlyContent}">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
export default {
  props : {
    selectType : {
      type : Number
    },
    onlyContent : {
      type : Boolean
    }
  },
  methods : {
    select (type,event) {
       if (!event._constructed) {
          return;
        }
        this.$emit('select',type)
    },
     toggleContent(event){
       if (!event._constructed) {
          return;
        }
        this.$emit('toggle')
        console.log(this.onlyContent)
     }
  }
}
</script>

<style lang="stylus" ref="stylesheet/stylus">
@import '../assets/stylus/index.styl';
#ratingselect
  margin-top 24px
  .rating-type
    padding-bottom 18px
    border-1px(rgba(77,85,93,.2))
    .all,.some,.bad
      padding 8px
      font-size 12px
      border-radius 2px
      line-height 16px
      margin-right 8px
    .all
      color #4d555d
      background-color rgba(0,160,220,.2)
      &.active
        background-color rgb(0,160,220)
        color rgb(255,255,255)
    .some
      color rgb(77,85,93)
      line-height 16px
      background-color rgba(0,160,220,.2)
      &.active
        background: rgb(0, 160, 220)
    .bad
      background-color rgba(77,85,93,.2)
      &.active
        background: rgb(77, 85, 93)
        color rgb(255,255,255)
  .switch
    border-bottom 1px solid rgba(77,85,93,.2)
    padding 12px 0
    &.on
      .icon-check_circle
          color: #00c850
    .icon-check_circle
      font-size 24px
      color rgb(147,153,159)
      line-height 24px
    .text
      font-size 12px
      color rgb(147,153,159)
      line-height 24px


</style>
