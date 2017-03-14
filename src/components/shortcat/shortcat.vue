<template>
  <div class="shortcat">
    <div class="content">
      <div class="content-l">
        <div class="logo_wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-r">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      selectFoods: {
        type: Array,
        default() {
          return [{
            price: 10,
            count: 1
          }];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    computed: {
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        } else {
          return '去结算';
        }
      },
      payClass() {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'engough';
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .shortcat
    position: fixed
    left: 0
    bottom: 0
    z-index 50
    height: 48px
    width: 100%
    .content
      display flex
      background: #141d27
      font-size 0
      .content-l
        flex 1
        .logo_wrapper
          display inline-block
          position relative
          top: -10px
          margin: 0 12px
          padding: 6px
          width: 56px
          height: 56px
          box-sizing border-box
          vertical-align top
          border-radius 50%
          background: #141d27
          .num
            position: absolute
            top: 0
            right: 0
            width: 24px
            height: 16px
            line-height: 16px
            text-align center
            border-radius 16px
            font-size 9px
            font-weight 700
            color: #fff
            background: rgb(240, 20, 20)
            box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
          .logo
            width: 100%
            height: 100%
            border-radius 50%
            text-align center
            background: #2b343c
            &.highlight
              background: rgb(0, 160, 220)
            .icon-shopping_cart
              line-height: 44px
              font-size 24px
              color: #80858a
              &.highlight
                color: #fff
        .price
          display inline-block
          vertical-align top
          font-size 16px
          margin-top 12px
          padding-right: 12px
          font-weight 700
          line-height: 24px
          box-sizing border-box
          border-right 1px solid rgba(255, 255, 255, 0.1)
          color: rgba(255, 255, 255, 0.4)
          &.highlight
            color: #fff
        .desc
          display inline-block
          vertical-align top
          margin 12px 0 0 12px
          line-height: 24px
          color: rgba(255, 255, 255, 0.4)
          font-size 10px
      .content-r
        flex 0 0 105px
        width: 105px
        .pay
          height: 48px
          line-height: 48px
          font-weight 700
          color: rgba(255, 255, 255, 0.4)
          font-size 12px
          text-align center
          background: #2b333b
          &.not-enough
            background: #2b333b
          &.engough
            background: #00b43c
            color: #fff
</style>
