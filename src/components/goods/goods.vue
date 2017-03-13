<template>
  <div class="goods">
    <div class="goods_l">
      <ul class="menu_wrapper">
        <li class="menu-item" v-for="item in goods">
          <span class="text border-1px">
            <span class="icon" v-if="item.type>0" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="goods_r"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 0;

  export default {
    props: {
      seller: {}
    },
    data() {
      return {
        goods: {}
      };
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          console.log(this.goods);
        }
      });
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl';
  .goods
    display flex
    position: absolute
    top: 176px
    bottom: 46px
    width: 100%
    overflow hidden
    .goods_l
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display table
        height: 54px
        width: 56px
        line-height: 14px
        padding:0 12px
        .text
          display table-cell
          vertical-align middle
          text-align center
          width: 56px
          font-size 12px
          border-1px(rgba(7,17,27,0.1))
          .icon
            display inline-block
            height: 12px
            width: 12px
            vertical-align top
            margin-right 2px
            background-size 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('decrease_3')
            &.discount
              bg-image('discount_3')
            &.guarantee
              bg-image('guarantee_3')
            &.invoice
              bg-image('invoice_3')
            &.special
              bg-image('special_3')
    .goods_r
      flex: 1
</style>
