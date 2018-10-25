<template>
  <div id="shopcar">
      <div class="control-wrapper">
          <span class="shopcar-btn">
              <span class="shop-car-icon icon-shopping_cart"></span>
          </span>
          <span class="shopcar-des">
            <span class="shopcar-all-money"></span>
            <span class="shopcar-line"></span>
            <span class="shopcar-delivery-money"></span>
          </span>
          <span class="shopcar-buy-btn"></span>
      </div>
      <div class="shopcar-list">
      </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "shopcar",
  data() {
    return {
    };
  }
};
</script>
<style lang="stylus" scoped>
@import '../assets/stylus/index.styl'
@import '../assets/stylus/style.css'

#shopcar
	position absolute
	bottom 0
	left 0
	width 2


</style>

