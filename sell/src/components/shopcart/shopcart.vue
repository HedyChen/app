<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo">
            <i class="icon-shopping_cart"></i>
          </div>
        </div>
        <div class="price">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay">
          ￥{{minPrice}}起送
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default{
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
            .icon-shopping_cart
              font-size 24px
              color #80858a
              line-height 44px
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
        background #2b333b
        .pay
          height 48px
          line-height 48px
          text-align center
          font-weight 700
          font-size 12px
</style>
