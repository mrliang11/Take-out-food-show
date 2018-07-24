<template>
  <transition name="move">
    <div class="food" ref="food" v-show="showFlag">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image" alt="">
          <div class="close-wrapper" @click="back">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="info-content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="sell-count">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="newPrice">￥{{food.price}}</span>
            <span class="oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="buy" @click.stop.prevent="addFirst" v-show="!food.count || food.count===0">
            加入购物车
          </div>
        </div>
        <split v-show="food.info"></split>
        <div class="food-info" v-show="food.info">
          <h1 class="title">商品介绍</h1>
          <span class="text">{{food.info}}</span>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect @toggle="toggleContent" @select="selectRating" :selectType = "selectType" :onlyContent = "onlyContent"></ratingselect>
          <div class="rating-wrapper">
            <ul>
              <!-- <li v-for="(rating,index) in food.ratings" :key="index" class="rating-item border-1px"> -->
              <li v-show="needShow(selectType,rating.rateType,rating.text)" v-for="(rating,index) in food.ratings" :key="index" class="rating-item border-1px">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" :src="rating.avatar" alt="">
                </div>
                <div class="time">{{rating.rateTime}}</div>
                <p class="text">
                  <span :class="{'icon-thumb_up': rating.rateType === 0,'icon-thumb_down': rating.rateType === 1}"></span>
                  <span v-show="rating.text">{{rating.text}}</span>
                </p>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue'
import BScroll from 'better-scroll'
import split from './split'
import ratingselect from './ratingselect'
export default {
  props:{
    food : {
      type : Object
    }
  },
  data(){
    return{
      showFlag : false,
      selectType: 2,
      onlyContent : true
    }
  },
  components : {
    split,
    ratingselect
  },
  methods : {
    show(){
      this.showFlag = true
      this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
    },
    back(){
      this.showFlag = false
    },
    needShow(selectType,type,text){
      if(this.onlyContent && !text) {
        return false;
      }
      if(this.selectType === 2){
        return true
      }else{
        return type === this.selectType;
      }
    },
    selectRating(type){
      this.selectType = type
      this.$nextTick(() => {
          this.scroll.refresh()
        })
    },
    toggleContent(){
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
          this.scroll.refresh()
        })
    },
    addFirst(event){
      if(!event._constructed){
        return
      }
      this.$emit('add',event.target)
      Vue.set(this.food,'count',1)
    }
  }
}
</script>

<style lang="stylus" ref="stylesheet/stylus">
@import '../assets/stylus/index.styl';
  .food
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background: #fff
    transform: translate3d(0, 0, 0)
    &.move-enter-active, &.move-leave-active
      transition: all 0.2s linear
    &.move-enter, &.move-leave-active
      transform: translate3d(100%, 0, 0)
    .image-header
      width 100%
      height 375px
      img
        width 100%
        height 100%
      .close-wrapper
        position: absolute
        top: 10px
        left: 0
        i
          font-size 20px
          color #fff
    .info-content
      position relative
      padding 18px
      .title
        font-size 14px
        font-weight 700
        color rgb(7,17,27)
        line-height 14px
        padding-bottom 8px
      .detail
        .sell-count
          font-size 10px
          color rgb(147,153,159)
          line-height 10px
          padding-bottom 18px
      .price
        .newPrice
          font-size 14px
          font-weight 700
          color rgb(240,20,20)
          line-height 24px
        .oldPrice
          font-size 10px
          font-weight 700
          color rgb(147,153,159)
          line-height 24px
          text-decoration line-through
      .buy
        position: absolute
        right: 18px
        bottom 18px
        width 74px
        height 24px
        border-radius 24px
        background-color rgb(0,160,220)
        font-size 10px
        color rgb(255,255,255)
        line-height 24px
        text-align center
    .food-info,.rating
      box-sizing border-box
      padding 18px
      .title
        font-size 14px
        font-weight 500
        color rgb(7,17,27)
        line-height 14px
        padding-bottom 8px
      .text
        font-size 12px
        font-weight 200
        color rgb(77,85,93)
        line-height 24px
      .rating-wrapper
        .rating-item
          position relative
          box-sizing border-box
          padding 16px 0
          border-bottom 1px solid rgba(77,85,93,.2)
          .user
            position absolute
            right 0
            top 16px
            .name
              font-size 10px
              color rgb(147,153,159)
              line-height 12px
            .avatar
              width 12px
              height 12px
              border-radius 50%
              margin-left 6px
          .time
            font-size 10px
            color rgb(147,153,159)
            line-height 12px
            margin-bottom 6px
          .text
            font-size 12px
            color rgb(7,17,27)
            line-height 16px
            .icon-thumb_up,.icon-thumb_down
              fontsize 12px
              line-height 24px
            .icon-thumb_up
              color rgb(0,160,220)
            .icon-thumb_down
              color rgb(147,153,159)






</style>

