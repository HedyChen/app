<template>
  <div class="seller" id="seller-scroll">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li v-for="(item, index) in seller.supports" class="support-item">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" id="picWrapper">
          <ul class="pic-list" id="picList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import star from '../star/star.vue';
  import split from '../split/split.vue';
  import Bscroll from 'better-scroll';
  export default {
    data () {
      return {
        favorite: false
      };
    },
    computed: {
      favoriteText () {
        return this.favorite ? '已收藏' : '收藏';
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    watch: {
      seller () {
        this.$nextTick(function () {
          this._initScroll();
          this._initPics();
        });
      }
    },
    mounted () {
      this.$nextTick(function () {
        this._initScroll();
        this._initPics();
      });
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      star,
      split
    },
    methods: {
      _initScroll () {
        if (!this.scroll) {
          this.scroll = new Bscroll(document.getElementById('seller-scroll'), {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      },
      _initPics () {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let Width = (picWidth + margin) * this.seller.pics.length - margin;
          document.getElementById('picList').style.width = Width + 'px';
          if (!this.picScroll) {
            this.picScroll = new Bscroll(document.getElementById('picWrapper'), {
              scrollX: true,
              eventPassthrough: 'vertical'
            });
          } else {
            this.picScroll.refresh();
          }
        }
      },
      toggleFavorite () {
        if (!event._constructed) {
          return;
        } else {
          this.favorite = !this.favorite;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="styesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .seller
    position absolute
    top 174px
    bottom 0px
    width 100%
    overflow hidden
    .overview
      position relative
      padding 18px
      .title
        line-height 14px
        margin-bottom 8px
        color rgb(7,17,27)
        font-size 14px
      .desc
        padding-bottom 18px
        font-size 0
        border-1px(rgba(7,17,27,0.1))
        .star
          display inline-block
          margin-right 8px
          vertical-align top
        .text
          margin-right 12px
          vertical-align top
          line-height 18px
          font-size 10px
          color rgb(7,17,27)
          display inline-block
      .remark
        display flex
        padding-top 18px
        .block
          flex 1
          text-align center
          border-right 1px solid rgba(7,17,27,0.1)
          &:last-child
            border none
          h2
            font-size 10px
            color rgb(127,153,159)
            margin-bottom 4px
            line-height 10px
          .content
            font-size 0px
            color rgb(7,17,27)
            line-height 24px
            .stress
              font-size 14px
      .favorite
        position absolute
        right 11px
        top 18px
        width 60px
        text-align center
        .icon-favorite
          display block
          line-height 24px
          font-size 24px
          color #d4d4d9
          margin-bottom 4px
          &.active
            color rgb(240,20,20)
        .text
          line-height 10px
          font-size 10px
          color rgb(77,85,93)
    .bulletin
      padding 18px 18px 0 18px
      .title
        margin-bottom 8px
        font-size 14px
        line-height 14px
        color rgb(7,17,27)
      .content-wrapper
        padding 0 12px 16px 12px
        border-1px(rgba(7,17,27,0.1))
        .content
          font-size 12px
          text-align justify
          color rgb(240,20,20)
          line-height 24px
      .supports
        .support-item
          font-size 0
          border-1px(rgba(7,17,27,0.1))
          padding 16px 0px
          margin 0 18px
          &:last-child
            border-none()
          .icon
            display inline-block
            width 16px
            height 16px
            vertical-align top
            margin 0px 6px 0px 8px
            background-size 16px 16px
            background-repeat no-repeat
            &.decrease
              bg-img('decrease_4')
            &.discount
              bg-img('discount_4')
            &.guarantee
              bg-img('guarantee_4')
            &.invoice
              bg-img('invoice_4')
            &.special
              bg-img('special_4')
          .text
            font-size 12px
            line-height 16px;
            color rgb(7,17,27)
            vertical-align top
    .pics
      padding 18px
      .title
        line-height 14px
        margin-bottom 8px
        color rgb(7,17,27)
        font-size 14px
        padding-bottom 12px
        border-1px(rgba(7,17,27,0.1))
      .pic-wrapper
        overflow hidden
        white-space nowrap
        width 100%
        .pic-list
          font-size 0
          .pic-item
            margin-right 6px
            width 120px
            height 90px
            display inline-block
            &:last-child
              margin-right 0
    .info
      padding 18px 18px 0 18px
      .title
        padding-bottom 8px
        font-size 14px
        line-height 14px
        color rgb(7,17,27)
        border-1px(rgba(7,17,27,0.1))
      .info-item
        font-size 12px
        line-height 16px
        color rgb(7,17,27)
        padding 16px 12px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
</style>
