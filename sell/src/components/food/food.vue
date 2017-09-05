<template>
  <transition name="fade">
    <div v-show="showFlag" class="food" id="scroll">
      <div>
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
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings" @set-selectType="setType" @set-onlyContent="setContent"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-for="rating in food.ratings" v-show="needShow(rating.rateType, rating.text)" class="rating-item">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avarta" :src="rating.avatar"/>
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <p class="text">
                  <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import Bscroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import {formatDate} from '../../common/js/date.js';
  import Vue from 'vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingSelect/ratingSelect.vue';

  const ALL = 2;
  // 所有的评价

  export default {
    data () {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
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
        this.selectType = ALL;
        this.onlyContent = false;
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
      },
      needShow (rateType, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return this.selectType === rateType;
        }
      },
      setType (typeValue) {
        this.selectType = typeValue;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      setContent (contentValue) {
        this.onlyContent = contentValue;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    },
    filters: {
      formatDate (time) {
        let date = new Date(time);
        return formatDate(date, 'yy-MM-dd hh:mm');
      }
    }
  };
</script>

<style lang="stylus" rel="styesheet/stylus">
  @import "../../common/stylus/mixin.styl"
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
      .sell-count
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
  .rating
    padding-top 18px
    .title
      line-height 14px
      font-size 14px
      color rgb(7,17,27)
      margin-left 18px
    .rating-wrapper
      padding 0 18px
      .no-rating
        padding 16px 0
        font-size 12px
        color rgb(147,153,159)
    .rating-item
      position relative
      border-bottom 1px solid rgba(7,17,27,0.1)
      padding 16px 0
      .user
        position absolute
        top 16px
        font-size 0px
        line-height 12px
        right 16px
        .name
          vertical-align top
          font-size 10px
          margin-right 6px
          color rgb(147,153,159)
          display inline-block
        .avarta
          width 12px
          height 12px
          border-radius 50%

      .time
        line-height 12px
        color rgb(147,153,159)
        margin-bottom 6px
        font-size 10px
      .text
        line-height 16px
        font-size 12px
        color rgb(7,17,27)
        .icon-thumb_up,.icon-thumb_down
          margin-right 4px
          line-height 16px
          font-size 12px
        .icon-thumb_up
          color rgb(0,160,220)
        .icon-thumb_down
          color rgb(147,153,159)
</style>
