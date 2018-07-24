<template>
  <div id="cart-control">
    <transition name="fade">
      <span class="decrease icon-remove_circle_outline" v-show="food.count > 0" @click.stop="remove"></span>
    </transition>
    <transition name="fade">
      <span class="count" v-show="food.count > 0" >{{food.count}}</span>
    </transition>
    <span class="increase icon-add_circle" @click.stop="add"></span>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  props : {
    food : {
      type : Object
    }
  },
  methods : {
    add(event){
        if(!this.food.count){
          Vue.set(this.food,"count")
          this.food.count = 1
        }else{
          this.food.count ++
        }
    },
    remove(event){
        if(this.food.count === 0){
          return
        }
        this.food.count --
    }
  }
}
</script>

<style lang="stylus" ref="styiesheet/stylus">
  @import '../assets/stylus/index.styl';
  #cart-control
    font-size 0
    .icon-remove_circle_outline,.icon-add_circle
      display inline-block
      width 24px
      height 24px
      line-height 24px
      font-size 24px
    .decrease,.increase
      padding 6px
      display inline-block
      line-height 24px
      color #00a0dc
      font-size 24px
    .count
      display inline-block
      text-align center
      width 12px
      font-size 10px
      color rgb(147,153,159)
      line-height 24px
      padding-top 6px
      vertical-align top
      font-weight 600
    .fade-enter-active, .fade-leave-active {
      transition: opacity .3s;
    }
    .fade-enter, .fade-leave-to  {
      opacity: 0;
    }


</style>
