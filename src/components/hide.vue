<template>
  <div id="hide" v-show="showInfo">
    <div class="star-info">
      <div class="info">{{seller.name}}</div>
      <div class="star">
        <star :score = "seller.score"></star>
      </div>
  </div>
  <div class="font-info">
    <div class="discount">
      <div class="discount-line">
        <span class="line"></span>
        <span class="discount-font">优惠信息</span>
        <span class="line"></span>
      </div>
      <div class="discount-icon">
        <div class="discount-icon-item" v-for="(suport,index) in seller.supports" :key="index">
          <span class="icon" :class="classMap[suport.type]"></span>
          <span class="discount-icon-item-font">{{suport.description}}</span>
        </div>
      </div>
    </div>
    <div class="notice">
      <div class="discount-line">
        <span class="line"></span>
        <span class="discount-font">商家信息</span>
        <span class="line"></span>
      </div>
      <div class="business-info">
        {{seller.bulletin}}
      </div>
    </div>
  </div>
  <div class="business">
    <span class="icon-close" @click="hide"></span>
  </div>
  </div>
</template>

<script>
import star from './star'
import { mapState } from 'vuex'
export default {
  props : {
    seller : {
      type : Object
    }
  },
  methods : {
    hide(){
      this.$store.state.showInfo = false
    }
  },
  // data(){
  //   return{
  //     show : this.$store.state.showInfo
  //   }
  // },
  computed : mapState(['showInfo']),
  components : {
    star
  },
  created () {
    this.classMap = ['decrease','discount','special','invoice','guarantee']
  }
}
</script>

<style lang='stylus' ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl';
@import '../../static/style.css';
#hide
  position fixed
  top: 0
  right: 0
  left: 0
  bottom: 0
  width 100%
  height 100%
  background-color rgba(7,17,27,.8)
  z-index 999
  // filter blur(10px)
  .star-info
    width 100%
    height 148px
    text-align center
    .info
      font-size 16px
      font-weight 700
      color rgb(255,255,255)
      line-height 16px
      padding-top 64px
    .star
      margin-top 16px
  .font-info
    flex: 1;
    padding: 0 28px;
    .discount,.notice
      .discount-line
        width 100%
        text-align center
        .line
          display inline-block
          width 112px
          height 1px
          background-color rgba(255,255,255,.2)
          vertical-align middle
        .discount-font
          padding 0 12px
          font-size 14px
          font-weight 700
          color rgb(255,255,255)
          line-height 14px
      .discount-icon
        padding-top 24px
        width: 100%;
        padding: 24px 12px 28px;
        .discount-icon-item
          font-size 12px
          font-weight 200
          color rgb(255,255,255)
          line-height 12px
          .icon
            display inline-block
            vertical-align top
            width 16px
            height 16px
            background-size 16px 16px
            margin-bottom 12px
            margin-right 6px
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
    .notice
     .business-info
      padding: 24px 12px 0;
      font-size 12px
      font-weight 200
      color rgb(255,255,255)
      line-height 24px
  .business
    position absolute
    bottom 32px
    width 100%
    text-align center
    span
      font-size 32px
      color rgba(255,255,255,.5)
</style>
