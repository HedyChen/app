<template>
 <div class="ratings" id="rating-scroll">
   <div>
     <div class="rating-content">
       <div class="overview">
         <div class="overview-left">
           <h1 class="sroll">{{seller.score}}</h1>
           <div class="title">综合评分</div>
           <div class="rank">高于周边商家{{seller.rankRate}}%</div>
         </div>
         <div class="overview-right">
           <div class="score-wrapper">
             <span class="title">服务态度</span>
             <star :size="36" :score="seller.serviceScore"></star>
             <span class="score">{{seller.serviceScore}}</span>
           </div>
           <div class="score-wrapper">
             <span class="title">商品评价</span>
             <star :size="36" :score="seller.foodScore"></star>
             <span class="score">{{seller.foodScore}}</span>
           </div>
           <div class="delivery-wrapper">
             <span class="title">送达时间</span>
             <span class="delivery">{{seller.deliveryTime}}分钟</span>
           </div>

         </div>
       </div>
     </div>
     <split></split>
     <ratingSelect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="ratings" @set-selectType="setType" @set-onlyContent="setContent"></ratingSelect>
     <div class="rating-wrapper">
       <ul v-show="ratings && ratings.length">
         <li v-for="rating in ratings" class="rating-item" v-show="needShow(rating.rateType, rating.text)">
           <div class="avartar">
             <img width="28" height="28" :src="rating.avatar"/>
           </div>
           <div class="content">
             <h1 class="name">{{rating.username}}</h1>
             <div class="star-wrapper">
               <star :size="24" :scroe="rating.score"></star>
               <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
             </div>
             <p class="text">{{rating.text}}</p>
             <div class="recommend" v-show="rating.recommend && rating.recommend.length">
               <span class="icon-thumb_up"></span>
               <span v-for="item in rating.recommend" class="item">{{item}}</span>
             </div>
             <div class="time">{{rating.rateTime | formatDate}}</div>
           </div>
         </li>
       </ul>
     </div>
   </div>
 </div>
</template>

<script>
  import star from '../star/star.vue';
  import split from '../split/split.vue';
  import ratingSelect from '../ratingSelect/ratingSelect.vue';
  import Bscroll from 'better-scroll';
  import {formatDate} from '../../common/js/date.js';

  const ALL = 2;
  const ERR_OK = 0;
  export default{
    props: {
      seller: {
        type: Object,
        default: {}
      }
    },
    data () {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      };
    },
    created () {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.ratings = response.data;
          this.$nextTick(() => {
            this.scroll = new Bscroll(document.getElementById('rating-scroll'), {
              click: true
            });
          });
        };
      });
    },
    components: {
      star,
      split,
      ratingSelect
    },
    methods: {
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
      }
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
  .ratings
    position absolute
    top 174px
    bottom 0px
    width 100%
    overflow hidden
    .overview
      display flex
      padding 18px 0
      .overview-left
        flex 0 0 137px
        width 137px
        border-right 1px solid rgba(7,17,27,0.1)
        text-align center
        padding 6px 0;
        @media only screen and (max-width:320px)
          flex 0 0 120px
          width 120px
        .sroll
          font-size 24px
          color rgb(255,153,0)
          line-height 28px
          margin-bottom 6px
        .title
          color rgb(7,17,27)
          font-size 12px
          line-height 12px
          margin-bottom 8px
        .rank
          color rgb(147,153,159)
          line-height 10px
          font-size 10px
      .overview-right
        flex 1
        padding 6px 0 6px 24px
        vertical-align middle
        @media only screen and (max-width:350px)
          padding-left 6px
        .score-wrapper
          font-size 0px
          .title
            display inline-block
            vertical-align top
            font-size 12px
            color rgb(7,17,27)
            margin-right 12px
            line-height 18px
            @media only screen and (max-width:350px)
              margin-right 8px
          .star
            display inline-block
            vertical-align top
            line-height 18px
          .score
            vertical-align top
            color rgb(255,153,0)
            margin-left 12px
            line-height 18px
            font-size 12px
            @media only screen and (max-width:350px)
              margin-left 8px
        .delivery-wrapper
          font-size 0px
          .title
            display inline-block
            vertical-align top
            font-size 12px
            color rgb(7,17,27)
            margin-right 12px
            line-height 18px
            @media only screen and (max-width:350px)
              margin-right 8px
          .delivery
            vertical-align top
            color rgb(147,153,159)
            line-height 18px
            font-size 12px
  .rating-wrapper
    padding 0 18px
    .rating-item
      display flex
      padding 18px 0
      border-1px(rgba(7,17,27,0.1))
      .avartar
        flex 0 0 28px
        width 28px
        margin-right 12px
        img
          border-radius 50%
      .content
        position relative
        flex 1
        .name
          margin-bottom 4px
          color rgb(7,17,27)
          line-height 12px
          font-size 10px
        .star-wrapper
          font-size 0
          margin-bottom 6px
          .star
            vertical-align top
            margin-right 16px
            display inline-block
          .delivery
            vertical-align top
            font-size 10px
            color rgb(147,153,159)
            display inline-block
        .text
          color rgb(7,17,27)
          font-size 12px
          margin-bottom 8px
          line-height 18px
        .recommend
          font-size 0
          line-height 16px
          .icon-thumb_up,.item
            margin 0 8px 4px 0
            font-size 9px
            display inline-block
          .icon-thumb_up
            color rgb(0,160,220)
          .item
            padding 0 6px
            border-radius 1px
            color rgb(147,153,159)
            background #fff
            border 1px solid rgba(7,17,27,0.1)

        .time
          position absolute
          top 0
          right 0px
          line-height 12px
          color rgb(147,153,159)
          font-size 10px
</style>
