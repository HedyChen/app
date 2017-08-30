<template>
  <transition name="fade">
    <div v-show="showFlag" class="food">
      <div class="image-header">
        <img :src="food.image"/>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
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
      }
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
  .fade-enter-active, .fade-leave-active
    transition: transform .8s
</style>
