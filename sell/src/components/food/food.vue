<template>
  <transition name="fade">
    <div v-show="showFlag" class="food" id="scroll">
      <div class="image-header">
        <img :src="food.image"/>
        <div class="back" @click="hide">
          <i class="icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <h1 class="title">{{food.name}}</h1>
        <div class="food-detail">
          <span class="sell-count">月售{{food.sellCount}}份</span>
          <span class="rating">好评率{{food.rating}}%</span>
        </div>
        <div class="price">
          <span class="now">￥{{food.price}}</span><span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontrol-wrapper">
          <cartcontrol :food="food"></cartcontrol>
        </div>
        <div class="buy" v-show="!food.count || food.count==0" @click.stop.prevent="addFirst">加入购物车</div>
      </div>
      <split v-show="food.info"></split>
      <div class="info" v-show="food.info">
        <h1 class="title">商品信息</h1>
        <p class="text">{{food.info}}</p>
      </div>
      <div class="info" v-show="food.info">
        <h1 class="title">商品信息</h1>
        <p class="text">{{food.info}}</p>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import Bscroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import Vue from 'vue';
  import split from '../split/split.vue';
  export default {
    data () {
      return {
        showFlag: false
      };
    },
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      show () {
        this.showFlag = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new Bscroll(document.getElementById('scroll'), {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      hide () {
        this.showFlag = false;
      },
      addFirst (event) {
        if (!event._constructed) {
          return;
        }
        Vue.set(this.food, 'count', 1);
      }
    },
    components: {
      cartcontrol,
      split
    }
  };
</script>

<style lang="stylus" rel="styesheet/stylus">
  .food
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background #fff
    transform: translate3d(0,0,0)
  .fade-enter,.fade-leave-to
    transform: translate3d(100%,0,0)
  .fade-enter-active,.fade-leave-active
    transition: transform .8s
  .food .image-header
    position relative
    height 0
    width 100%
    padding-bottom 100%
    img
     position absolute
     left 0
     top 0
     width 100%
     height 100%
    .back
      position absolute
      left 0
      top 10px
    .icon-arrow_lift
      display block
      padding 10px
      font-size 20px
      color #fff
  .food .content
    padding 18px
    position relative
    .title
      font-size 14px
      margin-bottom 8px
      line-height 14px
      color rgb(7,17,27)
      font-weight 700
    .food-detail
      margin-bottom 18px
      line-height 10px
      font-size 0px
      height 10px
      .sell-count,.rating
        font-size 10px
        color rgb(147,153,159)
      &.sell-count
        margin-right 12px
    .price
      line-height 24px
      font-weight 700
      .now
        font-size 14px
        color rgb(240,20,20)
      .old
        font-size 10px
        text-decoration line-through
        color rgb(147,153,159)
    .cartcontrol-wrapper
      position absolute
      right 12px
      bottom 12px
    .buy
      position absolute
      right 12px
      bottom 18px
      z-index 10
      line-height 24px
      padding 0 12px
      box-sizing border-box
      font-size 10px
      border-radius 12px
      color #fff
      background rgb(0,160,220)
  .food .info
    padding 18px
    .title
      line-height 14px
      font-size 14px
      color rgb(7,17,27)
      margin-bottom 16px
    .text
      font-size 12px
      padding 0 8px
      color rgb(77,85,93)
      line-height 24px
</style>
