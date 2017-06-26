<template>
  <div id="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="descripton">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count">
      	<span class="count">{{seller.supports.length}}个</span>
      	<i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulluetin-wrapper">
      <span class="bulluetin-title"></span><span class="bulluetin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
    	<img :src="seller.avatar" width="100%" height="100%">
    </div>
  </div>
</template>

<script  type="text/ecmascript-6">
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  #header
    color: #fff
    position relative
    overflow hidden
    background rgba(7,17,27,0.5)
    .content-wrapper
      position relative
      padding 24px 12px 18px 24px
      font-size 0
      .avatar
        display inline-block
        vertical-align top
        img
        	border-radius 2px
      .content
        display inline-block
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            width 30px
            height 18px
            display inline-block
            vertical-align top
            bg-img('brand')
            background-size 30px 18px
          .name
            font-size 16px
            line-height 18px
            margin-left 6px
            vertical-align top
            font-weight bold
        .descripton
        	font-size 12px
        	line-height 12px
        	margin 0 0 10px 0
        .support
          .icon
            display inline-block
            width 12px
            height 12px
            vertical-align top
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-img('decrease_1')
            &.discount
              bg-img('discount_1')
            &.guarantee
              bg-img('guarantee_1')
            &.invoice
              bg-img('invoice_1')
            &.special
              bg-img('special_1')
          .text
            line-height 12px
            font-size 10px
            color #fff
	  .support-count
	  	position absolute
	  	right 12px
	  	bottom 14px
	  	padding 0 8px
	  	height 24px
	  	line-height 24px
	  	border-radius 14px
	  	background rgba(0,0,0,0.2)
	  	text-align center
	  	.count
	  		vertical-align top
	  		font-size 10px
	  		margin-left 2px
	  	.icon-keyboard_arrow_right
	  		line-height 24px
	  		margin-left 2px
	  		font-size 12px
	.bulluetin-wrapper
	  position relative
	  height 28px
	  line-height 28px
	  padding 0 22px 0 12px
	  white-space nowrap
	  overflow hidden
	  text-overflow ellipsis
	  background rgba(7,17,27,0.2)
	  .bulluetin-title
	  	vertical-align top
	  	display inline-block
	  	width 22px
	  	height 12px
	  	bg-img('bulletin')
	  	background-size 22px 12px
	  	background-repeat no-repeat
	  	margin-top 7px
	  .bulluetin-text
	  	vertical-align top
	  	font-size 10px
	  	margin 0 4px
	  .icon-keyboard_arrow_right
	  	position absolute
	  	font-size 12px
	  	right 12px
	  	top 10px
	.background
	  position absolute
	  top 0px
	  left 0
	  width 100%
	  height 100%
	  z-index -1
	  filter blur(10px)
</style>
