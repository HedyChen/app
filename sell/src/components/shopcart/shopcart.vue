<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper" @click="toggleList">
          <div class="logo" :class="{'highlight': totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight': totalCount>0}"></i>
          </div>
          <div class="number" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight': totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
    <transition name="fade">
      <div class="shorcart-list" v-show="listShow">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty">清空</span>
        </div>
        <div class="list-content">
          <ul>
            <li class="food" v-for="food in selectFoods">
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span>￥{{food.price * food.count}}</span>
              </div>
              <div class="cartcontrol_wrapper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  export default{
    data () {
      return {
        fold: true
      };
    },
    props: {
      selectFoods: {
        type: Array,
        default () {
          return [];
        }
      },
      deliveryPrice: {
        type: Number,
        defalut: 0
      },
      minPrice: {
        type: Number,
        defalut: 0
      }
    },
    computed: {
      totalPrice () {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount () {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        } else {
          return '去结算';
        }
      },
      payClass () {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      },
      listShow () {
        if (!this.totalCount) {
          this.fold = true;
          return false;
        };
        let show = !this.fold;
        return show;
      }
    },
    components: {
      cartcontrol
    },
    methods: {
      toggleList () {
        if (!this.totalCount) {
          return;
        };
        this.fold = !this.fold;
      }
    }
  };
</script>

<style lang="stylus" rel="styesheet/stylus">
  .shopcart
    position fixed
    z-index 50
    left 0
    bottom 0
    width 100%
    height 48px
    .content
      display flex
      background #14172d
      color rgba(255,255,255,0.4)
      .content-left
        flex 1
        .logo-wrapper
          position relative
          top -10px
          margin 0 12px
          padding 6px
          width 56px
          height 56px
          box-sizing border-box
          vertical-align top
          border-radius 50%
          background #14172d
          display inline-block
          .logo
            width 100%
            height 100%
            border-radius 50%
            background #2b343c
            text-align center
            &.highlight
              background rgb(0,160,220)
            .icon-shopping_cart
              font-size 24px
              color #80858a
              line-height 44px
              &.highlight
                color: rgb(255,255,255)
        .number
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          font-size 9px
          font-weight 700px
          color #fff
          border-radius 16px
          background rgb(240,20,20)
          box-shadow 0 4px 8px rgba(0,0,0,0.4)
        .price
          display inline-block
          font-weight bold
          vertical-align top
          line-height 24px
          font-size 16px
          box-sizing border-box
          padding-right 12px
          border-right 1px solid rgba(255,255,255,0.1)
          margin-top 12px
          &.highlight
            color: rgb(255,255,255)
        .desc
          display inline-block
          vertical-align top
          line-height 24px
          margin 12px 0 0 12px
          font-size 10px
          font-weight 300
      .content-right
        flex 0 0 105px
        width 105px
        .pay
          height 48px
          line-height 48px
          text-align center
          font-weight 700
          font-size 12px
          &.not-enough
            background #2b333b
          &.enough
            color #fff
            background #00b43c
    .shorcart-list
      position absolute
      z-index -1
      bottom 111px
      left 0
      width 100%
      .list-header
        height 40px
        background #f5f3f7
        line-height 40px
        padding 0 18px
        border-bottom 1px solid rgba(7,17,27,0.1)
        .title
          float left
          font-size 14px
          font-weight 100
          color rgb(7,17,27)
        .empty
          float right
          font-size 12px
          color rgb(0,160,200)
      .list-content
        height 48px
        line-height 48px
        margin 0 18px
        background #fff
    .fade-enter-active, .fade-leave-active
      transition all 0.3s linear
      opacity 1
    .fade-enter, .fade-leave-to
      opacity: 0
</style>
