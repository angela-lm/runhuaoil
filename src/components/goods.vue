<template>
  <div id="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul class="menu-list">
        <li class="menu" v-for="(menu,index) in goods" :key="index" @click="selectIndex(index,$event)" :class="{current: currentIndex === index}">
          <span class="menu-title border-1px">
            <span v-if="menu.type >= 0" class="menu-icon" :class="supports[menu.type]"></span>
            {{menu.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul class="foods-list">
        <li class="foods border-1px" v-for="(good,index) in goods" :key="index">
          <span class="goods-title">{{good.name}}</span>
          <ul class="goods-list">
            <li class="goods border-1px" v-for="goodItem in good.foods" :key="goodItem.id">
              <div class="goods-image">
                <img :src="goodItem.icon" class="set-img"/>
              </div>
              <div class="goods-des">
                <span class="goods-name">{{goodItem.name}}</span>
                <span v-if="goodItem.description" class="goods-description" @mouseenter='isShort="false"' @mouseleave='isShort="true"' >{{isShort === 'false' ? goodItem.description : shortStr(goodItem.description,20)}}</span>
                <span class="goods-details">
                  <span class="goods-sellcount">月售{{goodItem.sellCount}}份</span><span class="goods-rating">好评率{{goodItem.rating}}%</span>
                </span>
                <span class="goods-price">
                  <span class="goods-newprice"><span class="money">￥</span>{{goodItem.price}}</span>
                  <span class="goods-oldprice" v-if="goodItem.oldPrice">{{goodItem.oldPrice}}</span>
                </span>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue from "vue";
import betterScroll from "better-scroll";

export default {
  name: "goods",
  data() {
    return {
      goods: {},
      isShort: true,
      supports: ['decrease','discount','special','invoice','guarantee'],
      heightList: [],
      scrollY: 0
    };
  },
  created() {
    axios.get('/good/goods').then(
      res => {
        if(res.data.code === 0){
          this.goods = res.data.data;
          Vue.nextTick(() =>{
            // dom 绑定一个scroll事件
            // 计算一下每个foods的高度
            this._initscroll();
            this._caculateHeight();

          })
        }
      }
    )
  },
  computed: {
    currentIndex () {
      if(this.scrollY < this.heightList[1]){
        return 0;
      }
      for(var i = 1 ; i < this.heightList.length; i++){
        if(this.scrollY > this.heightList[i - 1] && this.scrollY <= this.heightList[i]){
          return i;
        }
      }
    }
  },
  methods: {
    shortStr(str,length) {
      if(str.length <= length){
        return str;
      }else{
        return str.slice(0,length) + '...';
      }
    },
    selectIndex($index,$event){
      if(!$event._constructed){
        return
      }
      let foodsList = this.$refs.foodsWrapper.getElementsByClassName('foods');
      this.foodsScroll.scrollToElement(foodsList[$index],300);
    },
    // better scroll
    _initscroll() {
      this.menuScroll = new betterScroll(this.$refs.menuWrapper,{
        click: true
      });
      this.foodsScroll = new betterScroll(this.$refs.foodsWrapper,{
        probeType: 3,
        click: true
      })
      this.foodsScroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
    _caculateHeight() {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('foods');
      let height = 0;
      this.heightList.push(height);
      for(let i = 0; i < foodList.length; i++){
        height += foodList[i].clientHeight;
        this.heightList.push(height);
      }
    }
  }
};
</script>
<style lang="stylus" scoped>

@import '../assets/stylus/index.styl'
@import '../assets/stylus/style.css'

#goods
  display flex
  position absolute
  top 184px
  bottom 48px
  left 0
  right 0
  overflow hidden
  .menu-wrapper
    width 80px
    flex 0 0 80px
    background-color #f3f5f7
    font-size 12px
    line-height 14px
    font-weight 200
    .menu-list
      .menu
        padding 0 12px
        display table
        height 54px
        width 100%
        box-sizing border-box
        align-items center
        &.current
          position relative
          margin-top -1px
          background-color:rgb(255,255,255)
          .menu-title
            border-1px(rgba(255,255,255,1))
            font-weight 400
        .menu-title
          display table-cell
          vertical-align middle
          text-align justify
          border-1px(rgba(7,17,27,.1))
          .menu-icon
            display inline-block
            width 14px
            height 14px
            vertical-align bottom
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
  .foods-wrapper
    flex-grow 1
    background-color rgb(255,255,255)
    .foods-list
      .foods
        .goods-title
          display block
          height 26px
          font-size 12px
          color rgb(147,153,159)
          line-height 26px
          text-indent 14px
          background-color #f3f5f7
          border-left 2px solid #d9dde1
        .goods-list
          .goods
            position relative
            display flex
            margin 0 18px
            padding 18px 0
            border-1px(rgba(7,17,27,.1))
            .goods-image
              flex-grow 80px
              width 80px;
              height 80px;
              vertical-align top
              img 
                set-img(80px);
            .goods-des
              flex-grow 1
              margin-left 10px
              vertical-align top
              >span
                display block
              .goods-name
                margin-top 2px
                font-size 14px
                color rgb(7,17,27)
                line-height 14px
              .goods-description
                margin-top 8px
                font-size 10px
                color rgb(147,153,159)
                line-height 12px
              .goods-details
                margin-top 8px
                font-size 10px
                color rgb(147,153,159)
                line-height 10px
                .goods-sellcount
                  margin-left 0
                .goods-rating
                  margin-left 12px
              .goods-price
                .goods-newprice
                  font-size 14px;
                  color rgb(255,0,0)
                  font-weight 700
                  line-height 24px
                  .money
                    font-size 10px
                .goods-oldprice
                  font-size 10px
                  color rgb(147,153,159)
                  font-weight 700
                  line-height 24px
                  text-decoration line-through

</style>

