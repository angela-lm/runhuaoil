<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item"><router-link to="goods">商品</router-link></div>
      <div class="tab-item"><router-link to="ratings">评价</router-link></div>
      <div class="tab-item"><router-link to="seller">商家</router-link></div>
    </div>
    <div class="content">
      <router-view></router-view>
    </div>
    <shop-car></shop-car>  
  </div>
</template>
<script>
import header from './components/header'
import goods from './components/goods'
import shopCar from './components/shopcar'
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      seller: {},
    }
  },
  components:{
    'v-header': header,
    goods,
    'shop-car': shopCar
  },
  created() {
    axios.get('/good/seller').then(
      res => {
        if(res.data.code === 0){
          this.seller = res.data.data;
        }
      }
    )
  }
}
</script>

<style lang="stylus" ref="stylesheet/stylus">
@import 'assets/stylus/index'
#app
  .tab
    display flex
    height 40px
    line-height 40px
    border-1px(rgba(7,17,27,.1))
    .tab-item
      flex-grow 1
      a
        display block
        width 100%
        height 100%
        font-size 14px
        text-align center
        color rgb(77,85,93)
      a.active
        color rgb(240,20,20) !important

</style>
