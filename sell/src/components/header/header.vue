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
          <span class="icon"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="bulluetin-wrapper"></div>
  </div>
</template>

<script  type="text/ecmascript-6">
  export default{
    props: {
      seller: {
        type: Object
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  #header
    color: #fff
    background: #000
    .content-wrapper
      padding 24px 12px 18px 24px
      font-size 0
      .avatar
        display inline-block
        vertical-align top
        img
        	border-radius 2px
      .content
        display inline-block
        font-size 14px
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
        	
            

</style>
