<template>
  <div class="shopcart">
    <div class="content">
      <div class="chart-icon-wrapper">
        <div class="chart-icon icon-shopping_cart" @click="showTime" :class="{noZChart: totalCount != 0}"></div>
        <div class="total-count" v-show="totalCount != 0">{{totalCount}}</div>
      </div>
      <div class="deliver-fee">
        <div class="price border-1px-right" :class="{noZprice: totalCount != 0}">￥{{totalPrice}}</div>
        <div class="deliveryPrice">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="deliver-base" @click="flags" :class="{ok: inform === '去结算'}">
        {{inform}}
      </div>
    </div>
    <div class="cart-list-wrapper" v-show="showList">
      <div class="cart-gray"></div>
      <div class="cart-list">
        <div class="cart-list-header">
          <span class="cart-title">购物车</span>
          <span class="clear" @click="removeAll">清空</span>
        </div>
        <div class="item-wrapper">
          <div class="food-item" v-for="food in selectFoods" :key="food.id">
            <span class="food-title">{{food.name}}</span>
            <span class="food-price">{{food.price}}</span>
            <cartcontrol class="cart-control" :food = "food"></cartcontrol>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import cartcontrol from './cartcontrol'
import BScroll from 'better-scroll'

export default {
  components : {
    cartcontrol
  },
  props : {
    deliveryPrice : {
      type : Number
    },
    minPrice : {
      type : Number
    },
    selectFoods : {
      type: Array,
      default(){
        return [
          {
            count: 10,
            price: 2
          },
          {
            count: 1,
            price:20
          }
        ]
      }
    }
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      fold: true
    }
  },
  methods : {
    removeAll () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    },
    showTime(){
      if(this.selectFoods.length > 0){
        this.fold = !this.fold
      }
    },
    flags(){
      this.$store.state.flag = true
    }
  },
  computed : {
    // ...mapState(['flag']),
    totalCount(){
      let totalCount = 0
      this.selectFoods.forEach((food) => {
      totalCount += food.count
      })
      return totalCount
    },
    totalPrice(){
      let totalPriceNum = 0;
      this.selectFoods.forEach((food) => {
      totalPriceNum += food.price * food.count
      })
      return totalPriceNum
    },
    inform(){
      if(this.totalPrice === 0){
        return `￥${this.minPrice}`
      }else if(this.totalPrice > 0 && this.totalPrice < this.minPrice){
        return `还差￥${20 - this.totalPrice}起送`
      }else{
        return `去结算`
      }
    },
    showList(){
      if(!this.totalCount){
        this.fold = true
        return false
      }
      return !this.fold
    }
  },
  created () {
    axios.get('/good/seller').then(
      res => {
        if(res.data.code === 0){
        }
      }
    )

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='stylus' ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl';
  .shopcart
    position fixed
    width 100%
    height 48px
    bottom 0
    left 0
    z-index 300
    .content
      width 100%
      display flex
      background #141d27
      .chart-icon-wrapper
        flex 0 0 80px
        position relative
        .chart-icon
          position relative
          width 44px
          height 44px
          border-radius 44px
          border 6px solid #141d27
          background-color #2b333b
          margin-left 18px
          margin-top -10px
          font-size 24px
          color rgba(255,255,255,.4)
          line-height 44px
          text-align center
          &.noZChart
            background-color #00a0dc
            color #fff
        .total-count
          position absolute
          right 0px
          top -6px
          text-align center
          background red
          color #ffffff
          width 24px
          height 16px
          font-size 8px
          font-weight 700
          line-height 16px
          box-shadow 0 4px 8px 0 rgba(0,0,0,.4)
          border-radius 16px
      .deliver-fee
        flex 1
        padding 12px 0px 12px 12px
        height 24px
        .price
          display inline-block
          line-height 24px
          color rgba(255,255,255,.2)
          border-1px-right(rgba(255,255,255,.4))
          padding-right 12px
          &.noZprice
            color #fff
        .deliveryPrice
          display inline-block
          padding-left 12px
          font-size 12px
          color rgba(255,255,255,.4)
          line-height 20px
          font-weight 700
      .deliver-base
        flex 0 0 105px
        padding 0 8px
        font-size 12px
        text-align center
        line-height 48px
        color rgba(255,255,255,.1)
        font-weight 700
        background-color #2b333b
        &.ok
          background-color green
          color #fff
  .cart-list-wrapper
    position fixed
    top 0
    bottom 48px
    width 100%
    display flex
    flex-direction column
    z-index -1
    .cart-gray
      flex 1
      background-color rgba(7,17,27,.6)
    .cart-list
      width 100%
      position fixed
      bottom 48px
      left 0
      background #ffffff
      max-height 217px
      .cart-list-header
        height 40px
        line-height 40px
        padding 0 18px
        border-1px(rgba(7,17,27,.1))
        background-color #f3f5f7
        .cart-title
          font-size 14px
          font-weight 200
          color rgb(7,17,27)
        .clear
          position absolute
          right 18px
          font-size 12px
          color rgb(0,160,220)
      .item-wrapper
        overflow auto
        max-height 178px
        .food-item
          width 100%
          height 48px
          line-height 48px
          margin 0 18px
          border-1px(rgba(7,17,27,.1))
          .food-title
            font-size 14px
            color rgb(7,17,27)
          .food-price
            position absolute
            right 120px
            font-size 10px
          .cart-control
            position absolute
            right 18px
            top 6px
            display inline-block
</style>
