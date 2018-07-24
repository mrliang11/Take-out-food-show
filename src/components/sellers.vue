<template>
  <div class="sellers" ref="sellers">
    <div class="seller-content">
      <div class="sellers-header">
        <h1>{{seller.name}}</h1>
        <div class="desc">
          <star :score = 'seller.score'></star>
          <span class="margin">({{seller.ratingCount}})</span>
          <span>月销售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block border-1px">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content border-1px">
              <span class="stress">{{seller.deliveryPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>平均送达时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <span class="text" :class="{'active':favorite}">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="sellers-activity">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul class="supports">
          <li v-for="(item,index) in seller.supports" :key="index" class="support-item border-1px">
            <span class="icon" :class="classMap[item.type]"></span>
            <span class="text">{{item.description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="seller-view">
        <h1 class="title">商家信息</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="(pic,index) in seller.pics" :key="index">
              <img :src="pic" alt="">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="seller-info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="(item,index) in seller.infos" :key="index">
            {{item}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import star from './star'
import split from './split'
export default {
  props : {
    seller : {
      type : Object
    }
  },
  name: 'sellers',
  data () {
    return {
      msg : 'Welcome to this place',
      favorite : true
    }
  },
  components : {
    star,
    split
  },
  mounted(){
   this.$nextTick(() => {
      this._initScroll()
      this._initPic()
    });
  },
  created(){
    this.classMap = ['decrease','discount','special','invoice','guarantee']
  },
  watch: {
    'seller'() {
      this.$nextTick(() => {
        this._initScroll();
        this._initPic();
      });
    }
  },
  methods : {
    toggleFavorite(event){
      if(!event._constructed){
        return
      }
      this.favorite = !this.favorite
    },
    _initScroll(){
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.sellers, {
          click: true
        });
      } else {
        this.scroll.refresh();
        }
    },
    _initPic(){
      console.log(this.seller.pics)
      if(this.seller.pics){
        let picWidth = 120
        let margin = 6
        let Width = (picWidth + margin) * this.seller.pics.length - margin
        this.$refs.picList.style.width = Width + 'px'
        this.$nextTick(() => {
          if(!this.picScroll){
            this.picScroll = new BScroll(this.$refs.picWrapper,{
              scrollX : true,
              eventPassthrough: 'vertical'
            });
          }else{
            this.picScroll.refresh();
          }
        })
      }
    }
  },
  computed:{
    favoriteText(){
      return this.favorite ? '已收藏' : '收藏'
    }
  }
}
</script>

<style lang="stylus" ref="stylesheet/stylus">
@import '../../static/style.css';
@import '../assets/stylus/index.styl';
.sellers
  position: absolute
  top: 174px
  bottom: 0
  left: 0
  width: 100%
  overflow: hidden
  .seller-content
    .sellers-header
      position relative
      padding 18px 18px 0 18px
      h1
        font-size 14px
        color rgb(7,17,27)
        line-height 14px
        margin-bottom 8px
        margin-left 3px
      .desc
        padding-bottom 18px
        border-bottom 1px solid rgba(7,17,27,.1)
        .margin
           margin-right 6px
        #star
          height 18px
          line-height 18px
          margin-right 8px
          .star-img
            width 18px !important
            height 18px !important
            background-size: 18px 18px !important
            margin-left: 3px !important
        span
          vertical-align top
          font-size 10px
          color rgb(7,17,27)
          line-height 18px
      .remark
        display flex
        padding 18px 0
        .block
          flex 1
          padding 0 18px
          text-align: center
          border-1px-right(rgba(7,17,27,.1))
          h2
            font-size 10px
            color rgb(147,153,159)
            line-height 10px
            margin-bottom 4px
          .content
            .stress
              font-size 20px
              font-weight 200
              color rgb(7,17,27)
              line-height 20px
      .favorite
        position absolute
        right 18px
        top 15px
        width 37px
        text-align center
        .icon-favorite
          font-size 24px
          color #d4d6d9
          line-height 24px
          &.active
            color: rgb(240, 20, 20)
        .text
          font-size 10px
          color #d4d6d9
          line-height 10px
          &.active
            color rgb(240, 20, 20)
    .sellers-activity
      padding 18px 18px 0 18px
      .title
        font-size 14px
        color rgb(7,17,27)
        line-height 14px
        margin-bottom 8px
      .content-wrapper
        padding 0 12px 16px 12px
        border-bottom 1px solid rgba(7,17,27,.1)
        .content
          font-size 12px
          font-weight 200
          color rgb(240,20,20)
          line-height 24px
      .supports
        .support-item
          padding 16px 12px
          border-bottom 1px solid rgba(7,17,27,.1)
          &:last-child
            border none
          .icon
            display inline-block
            vertical-align top
            width 16px
            height 16px
            background-size 16px 16px
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
          .text
            font-size 12px
            font-weight 200
            color rgb(7,17,27)
            line-height 16px
    .seller-view
      padding 18px
      .title
        font-size 14px
        color rgb(7,17,27)
        line-height 14px
        margin-bottom 12px
      .pic-wrapper
        width: 100%
        overflow: hidden
        white-space: nowrap
        .pic-list
          font-size 0
          .pic-item
            display inline-block
            margin-left 6px
            width 120px
            height 90px
            img
              width 100%
              height 100%
    .seller-info
      padding 18px 18px 0 18px
      .title
        font-size 14px
        color rgb(7,17,27)
        line-height 14px
        padding-bottom 12px
        border-bottom 1px solid rgba(7,17,27,.1)
      .info-item
        padding 16px 12px
        font-size 12px
        font-weight 200
        color rgb(7,17,27)
        line-height 16px
        border-bottom 1px solid rgba(7,17,27,.1)
</style>
