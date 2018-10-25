<template>
  <div id="elm-header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" alt="set-img">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand bg-image"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="delivery">
          <span class="description">{{seller.description}}</span>
          <span class="delivery-time">/{{seller.deliveryTime}}分钟送达</span>
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="icon bg-image" :class="supports[seller.supports[0].type]"></span>
          <span class="s-description">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="supports-detail-show" v-if="seller.supports">{{seller.supports.length}}个 &gt;</div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-icon bg-image"></span>
      <span class="bulletin-content">{{seller.bulletin}}</span>
      <span class="bulletin-more">&gt;</span>
    </div>
    <div class="background-wrapper">
      <img :src="seller.avatar"/>
    </div>
  </div>
</template>

<script>
export default {
  name: "header",
  props:{
    seller: {
      type: Object
    }
  },
  data() {
    return {
      supports: ['decrease','discount','special','invoice','guarantee']

    };
  },
  computed: {
  },
  created() {
  }
};
</script>
<style lang="stylus" scoped>

@import '../assets/stylus/index.styl'
@import '../assets/stylus/style.css'

#elm-header
  position relative
  overflow hidden
  .content-wrapper
    position relative
    padding 24px 24px 18px 12px
    background rgba(7,17,27,.5)
    .avatar
      float left
      img
        border-radius 2px
        set-img(64px)
    .content
      padding 2px 0 2px 16px
      margin-left 64px
      margin-top 2px
      color rgb(255,255,255)
      .title
        .brand
          display inline-block
          width 30px
          height 18px
          bg-image('brand')
          background-size 30px 18px
          vertical-align bottom
        .name
          font-size 16px
          color rgb(255,255,255)
          font-weight bold
          line-height 18px
          vertical-align bottom
      .delivery
        padding-top 8px
        .delivery-time,.description
          font-size 12px
          rgb(255,255,255)
          font-weight 200
          line-height 12px
      .supports
        padding-top 10px
        .icon
          display inline-block
          width 14px
          height 14px
          background-size 14px 14px
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.special
            bg-image('special_1')
          &.guarantee
            bg-image('guarantee_1')
          &.invoice
            bg-image('invoice_1')
        .s-description
          font-size 10px
          color rgb(255,255,255)
          line-height 12px
    .supports-detail-show
      position absolute
      right 12px
      bottom 18px;
      font-size 10px
      color rgb(255,255,255)
      line-height 12px
      font-weight 200
      padding 7px 8px
      background-color rgba(0,0,0,.2);
      border-top-left-radius 14px
      border-top-right-radius 16px
      border-bottom-right-radius 16px
      border-bottom-left-radius 14px
  .bulletin-wrapper
    display flex
    justify-content center
    align-items center
    height 28px
    padding 0 12px
    font-size 10px
    color rgb(255,255,255)
    font-weight 200
    line-height 28px
    background-color rgba(7,17,27,.2)
    .bulletin-icon
      margin-right 4px
      padding 6px 12px
      bg-image('bulletin')
      background-size 24px 12px
    .bulletin-content
      flex-grow 1
      white-space nowrap
      overflow hidden
      text-overflow ellipsis 
    .bulletin-more
      font-size 10px
      color rgb(255,255,255)
      line-height 28px
  .background-wrapper
    position absolute
    top 0
    left 0
    right 0
    bottom 0
    z-index -1
    img 
      width 100%
      filter blur(10px)
</style>
