<template>
  <div class="cartcontrol">
    <transition name="fade">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart">
      <span class="inner icon-add_circle"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default{
    props: {
      food: {
        type: Object,
        default: {}
      }
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count ++;
        }
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count >= 0) {
          this.food.count --;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="styesheet/stylus">
  .cartcontrol
    font-size
    .cart-decrease
      display inline-block
      padding 6px
      .inner
        display inline-block
        line-height 24px
        font-size 24px
        color rgb(0,160,220)
        vertical-align top
    .cart-count
      display inline-block
      line-height 24px
      padding-top 6px
      width 12px
      text-align center
      font-size 10px
      color rgb(147,153,159)
      vertical-align top
    .cart-add
      display inline-block
      padding 6px
      line-height 24px
      font-size 24px
      color rgb(0,160,220)
      vertical-align top
    .fade-enter-active, .fade-leave-active
      transition all 0.3s linear
      transform translate3D(0,0,0)
      opacity 1
    .fade-enter, .fade-leave-to
      opacity: 0
      transform translate3D(24px,0,0)
</style>
