<template>
  <div class="goods">
    <div class="menu-wrapper" id="menu-scroll">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex==index}" @click="selectMenu(index,$event)">
          <span class="text border-1px"><span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" id="foods-scroll">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px border-none">
              <div class="icon">
                <img :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="all-count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartControl :food="food"></cartControl>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :select-foods="selectFoods"></shopcart>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
  import Bscroll from 'better-scroll';
  import shopcart from '../shopcart/shopcart.vue';
  import cartControl from '../cartcontrol/cartcontrol.vue';
  import food from '../food/food.vue';
  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
      },
      selectFoods () {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._caculateHeight();
          });
        }
      });
    },
    methods: {
      _initScroll () {
        this.menuScroll = new Bscroll(document.getElementById('menu-scroll'), {
          click: true
        });
        this.foodScroll = new Bscroll(document.getElementById('foods-scroll'), {
          probeType: 3,
          click: true
        });
        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _caculateHeight () {
        let foodList = this.$el.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu (index, event) {
        if (!event._constructed) {
          return;
        }
        let foodList = this.$el.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodScroll.scrollToElement(el, 300);
      },
      selectFood (food, event) {
        if (!event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      }
    },
    components: {
      shopcart,
      cartControl,
      food
    }
  };
</script>

<style lang="stylus" rel="styesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .goods
    display flex
    position absolute
    top 174px
    bottom 46px
    width 100%
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background #f3f5f7
      .menu-item
        display table
        height 54px
        width 56px
        padding 0 12px
        font-size 12px
        &.current
          position relative
          margin-top -1px
          z-index 10
          background #fff
          font-weight 700
          border-1px(#fff)
        .text
          display table-cell
          width 56px
          font-size 12px
          line-height 14px
          vertical-align middle
          border-1px(rgba(7,17,27,0.1))
          .icon
            display inline-block
            vertical-align top
            width 12px
            height 12px
            margin-right 2px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-img('decrease_3')
            &.discount
              bg-img('discount_3')
            &.guarantee
              bg-img('guarantee_3')
            &.invoice
              bg-img('invoice_3')
            &.special
              bg-img('special_3')
    .foods-wrapper
      flex 1
      .title
        height 26px
        line-height 26px
        border-left 2px solid #d9dde1
        font-size 12px
        color rgb(147,153,159)
        background #f3f5f7
        padding-left 14px
      .food-item
        display flex
        margin 18px 18px 0 18px
        padding-bottom 18px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
        .icon
          margin-right 10px
          flex 0 0 57px
          width 57px
          height 57px
          border-radius 2px
          img
            width 57px
            height 57px
        .content
          flex 1
          .name
            height 14px
            line-height 14px
            font-size 14px
            color rgb(7, 17, 27)
            margin 2px 0 8px 0
          .desc,.extra
            line-height 10px
            font-size 10px
            color rgb(147, 153, 159)
            margin-bottom 8px
            .all-count
              padding-right 12px
          .desc
            line-height 12px
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
            z-index 2
            bottom 0
            right 0
</style>
