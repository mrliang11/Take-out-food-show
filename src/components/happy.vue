<template>
  <div class="happy" v-show="flag">
    <div class="happy-content">
      <div class="happy-header">
        <span class="happy-header-info">提示</span>
        <span class="icon-close"></span>
      </div>
      <div class="happy-info">你是不是傻</div>
      <div class="happy-button">
        <div class="yes" @click="yes">是</div>
        <div class="no">否</div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  // data(){
  //   return {
  //     flag : this.$store.state.flag
  //   }
  // },
  methods :{
    yes(){
      this.$store.state.flag = false
    }
  },
  computed : mapState(['flag'])
}
</script>

<style lang="stylus" ref="stylesheet/stylus">
@import '../../static/style.css';
.happy
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(7,17,27,.5);
  z-index: 400;
  .happy-content
    position absolute
    left 50%
    top 50%
    transform translate(-50%,-50%)
    width 320px
    height 200px
    background-color #fff
    border-radius 5px
    .happy-header
      display flex
      box-sizing border-box
      justify-content space-between
      width 100%
      padding: 10px 20px
      background-color #f3f5f7
    .happy-info
      font-size: 20px;
      font-weight: 700;
      line-height: 100px;
      text-align center
    .happy-button
      display flex
      .yes,.no
        flex 1
        text-align center
        font-size 16px
        padding 12px
        margin: 0 8px;
        border: 1px solid rgba(7,17,27,.5);
</style>
