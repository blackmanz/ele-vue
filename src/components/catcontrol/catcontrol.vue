<template>
  <div class="catcontrol">
    <div class="cat-decrease" v-show="food.count>0" @click="decreaseCat" transition="move">
      <span class="inner icon-remove_circle_outline"></span>
    </div>
    <div class="cat-count" v-show="food.count>0">
      {{food.count}}
    </div>
    <div class="cat-add icon-add_circle" @click="addCat"></div>

  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCat(event) {
        if (!event._constructed) {
          return;
        }
//        console.log('click');
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        this.$dispatch('cart.add', event.target);
      },
      decreaseCat(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count--;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .catcontrol
    font-size 0
    .cat-decrease
      display inline-block
      padding: 6px
      transition: all 0.4s linear
      &.move-transition
        opacity: 1
        transform: translate3d(0, 0, 0)
        .inner
          display inline-block
          font-size 24px
          line-height: 24px
          color: rgb(0, 160, 220)
          transition: all 0.4s linear
          transform: rotate(0)
      &.move-enter, &.move-leave
        opacity 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cat-count
      display inline-block
      vertical-align top
      width: 12px
      font-size 10px
      padding-top 6px
      line-height: 24px
      text-align center
      color: rbg(147, 153, 159)
    .cat-add
      display inline-block
      padding: 6px
      font-size 24px
      line-height: 24px
      color: rgb(0, 160, 220)

</style>
