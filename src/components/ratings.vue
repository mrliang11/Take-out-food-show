<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="marking">
        <div class="marking-left border-1px">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="marking-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :score='seller.serviceScore' ></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :score='seller.foodScore' ></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <div class="padding">
        <ratingselect @toggle="toggleContent" @select="selectRating" :selectType = "selectType" :onlyContent = "onlyContent"></ratingselect>
        <div class="rating-wrapper">
          <ul>
            <li v-show="needShow(selectType,rating.rateType,rating.text)" v-for="(rating,index) in ratings" :key="index" class="rating-item border-1px">
              <div class="user">
                <img class="avatar" :src="rating.avatar" alt="">
              </div>
              <div class="flex">
                <div class="name-time">
                  <span class="name">{{rating.username}}</span>
                  <div class="time">2016-07-13 20:33</div>
                </div>
                <div class="star-wrapper">
                  <star :score='rating.score'></star>
                  <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
                </div>
                <p class="text">{{rating.text}}</p>
                <p class="class">
                  <span :class="{'icon-thumb_up': rating.rateType === 0,'icon-thumb_down': rating.rateType === 1}"></span>
                  <span class="item" v-for="(item,index) in rating.recommend" :key="index">{{item}}</span>
                </p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import star from './star'
import split from './split'
import ratingselect from './ratingselect'
export default {
  name: 'ratings',
  props: {
      seller: {
        type: Object
      }
    },
  data () {
    return {
      msg : 'Welcome to this place',
      ratings : [],
      selectType: 2,
      onlyContent : true
    }
  },
  components : {
    star,
    split,
    ratingselect
  },
  methods : {
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
  },
  created () {
    axios.get('/good/ratings').then(res => {
      if(res.data.code === 0){
        this.ratings = res.data.data
        this.$nextTick(()=>{
          this.scroll = new BScroll(this.$refs.ratings,{
            click : true
          })
        })
      }
    })
  }
}
</script>

<style lang="stylus" ref="stylesheet/stylus">
@import '../assets/stylus/index.styl';
.ratings
  position: absolute
  top: 174px
  bottom: 0
  left: 0
  width: 100%
  overflow: hidden
  .marking
    display flex
    box-sizing border-box
    padding 18px 0
    .marking-left
      flex 0 0 137.5px
      text-align center
      border-1px-right(rgba(7,17,27,0.1))
      .score
        font-size 24px
        color rgb(255,153,0)
        line-height 28px
        margin-bottom 6px
      .title
        font-size 12px
        color rgb(7,17,27)
        line-height 12px
        margin-bottom 8px
      .rank
        font-size 10px
        color rgb(7,17,27)
        line-height 10px
        margin-bottom 6px
    .marking-right
      flex 1
      box-sizing border-box
      padding 0 24px
      .score-wrapper,.delivery-wrapper
        height 18px
        line-height 18px
        .delivery
          font-size 12px
          color rgb(147,153,159)
          line-height 18px
          padding-left 10px
        .title
          display inline-block
          height 18px
          font-size 12px
          color rgb(7,17,27)
          line-height 20px
          vertical-align top
        #star
          height 18px
          padding 0 4px
          .star-img
            width 18px !important
            height 18px !important
            background-size: 18px 18px !important
            margin-left: 3px !important;
      .score-wrapper
        margin-bottom 8px
        .score
          font-size 12px
          color rgb(255,153,0)
          line-height 18px
          vertical-align top
  .padding
    padding 0 18px 18px 18px
    .rating-wrapper
      .rating-item
        display flex
        position relative
        box-sizing border-box
        padding 16px 0
        border-bottom 1px solid rgba(77,85,93,.2)
        .user
          width 28px
          height 28px
          border-radius 50%
          overflow hidden
          flex 0 0 28px
          margin-right 12px
          .avatar
            width 100%
            height 100%
        .flex
          width 299.2px
          flex 1
          .name-time
            display flex
            justify-content space-between
            margin-bottom 4px
            .name
              font-size 10px
              color rgb(7,17,27)
              line-height 12px
            .time
              font-size 10px
              font-weight 200
              color rgb(147,153,159)
              line-height 12px
          .star-wrapper
            margin-bottom 6px
            #star
              height 12px
              .star-img
                width 12px !important
                height 12px !important
                background-size: 12px 12px !important
                margin-left: 3px !important
            .delivery
              font-size 10px
              font-weight 200
              color rgb(147,153,159)
              line-height 12px
          .text
            font-size 12px
            color rgb(7,17,27)
            line-height 18px
            margin-bottom 8px
          .class
            overflow: hidden
            white-space nowrap
            text-overflow: ellipsis
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
            .item
              width: 40px
              font-size 9px
              color rgb(147,153,159)
              line-height 16px
              border 1px solid rgba(7,17,27,.1)
              border-radius 2px
              background-color rgb(255,255,255)
              padding 3px 6px
              margin-right 8px

  //     .user
  //       .name
  //         font-size 10px
  //         color rgb(147,153,159)
  //         line-height 12px
  //       .avatar
  //         float left
  //         width 28px
  //         height 28px
  //         border-radius 50%
  //         margin-left 6px
  //         &:after
  //           display block
  //           content ''
  //           clear both
  //     .time
  //       font-size 10px
  //       color rgb(147,153,159)
  //       line-height 12px
  //       margin-bottom 6px
  //     .text
  //       font-size 12px
  //       color rgb(7,17,27)
  //       line-height 16px
  //       .icon-thumb_up,.icon-thumb_down
  //         fontsize 12px
  //         line-height 24px
  //       .icon-thumb_up
  //         color rgb(0,160,220)
  //       .icon-thumb_down
  //         color rgb(147,153,159)
</style>
