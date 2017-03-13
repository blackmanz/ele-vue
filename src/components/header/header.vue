<template>
  <div class="header">
    <div class="header_t">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          <span>{{seller.description}}</span> /
          <span>{{seller.deliveryTime}}</span>分钟送达
        </div>
        <div class="supports" v-if="seller.supports">
          <span class="supports_l" :class="classMap[seller.supports[0].type]"></span>
          <span class="supports_r">{{seller.supports[0].description}}</span>
        </div>
        <div v-if="seller.supports" class="supports_square" @click="controlShow">
          <span class="supports_square_l">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
    </div>
    <div class="header_b" @click="controlShow">
      <span class="header_b_l"></span><span class="header_b_m">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="">
    </div>
    <div v-show="showDetail" class="detail" transition="fade">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="message">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul class="supports" v-if="seller.supports">
            <li class="supports-item" v-for="item in seller.supports">
              <span class="supports_l" :class="classMap[seller.supports[$index].type]"></span>
              <span class="supports_r">{{seller.supports[$index].description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="message">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="bulletin-p">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <i class="icon-close"></i>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../../components/star/star.vue';
  export default {
    props: {
      seller: {}
    },
    data() {
      return {
        showDetail: false
      };
    },
    methods: {
      controlShow() {
        this.showDetail = true;
      },
      hideDetail() {
        this.showDetail = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .header
    color: rgb(255, 255, 255)
    position: relative
    background: rgba(7, 17, 27, 0.5)
    overflow hidden
    .header_t
      padding: 24px 12px 18px 24px
      font-size: 0 // 这里font-size 设置为0 不让子元素继承自己的字体大小
      position: relative
      .avatar
        display: inline-block
        img
          border-radius: 2px
      .content
        display inline-block
        vertical-align: top
        font-size: 14px // 这里单独设置字体大小 让左右两边元素贴在一起
        margin-left: 16px
        .title
          margin: 2px 0 8px 0
          font-size: 0
          .brand
            display: inline-block
            height: 18px
            width: 30px
            bg-image('brand')
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            font-size: 16px
            line-height: 18px
            vertical-align: top
            margin-left: 6px
            font-weight: bold

        .description
          margin-bottom: 10px
          font-size: 12px
          line-height: 12px
        .supports
          line-height: 12px
          font-size: 0
          .supports_l
            margin-right: 4px
            height: 12px
            width: 12px
            display: inline-block
            background-size: 12px 12px
            background-repeat: no-repeat
            vertical-align: top
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .supports_r
            font-size: 10px
            line-height: 12px

        .supports_square
          position: absolute
          right: 12px
          bottom: 15px
          height: 24px
          padding: 0 8px
          border-radius: 14px
          background-color: rgba(0, 0, 0, 0.2)
          text-align: center
          .icon-keyboard_arrow_right
            font-size: 10px
            line-height: 24px
            vertical-align: middle
            margin-left: 2px
          .supports_square_l
            font-size: 10px
    .header_b
      padding: 0 22px 0 12px
      height: 28px
      background-color: rgba(7, 17, 27, 0.2)
      line-height: 28px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      position: relative
      .header_b_l
        display: inline-block
        height: 12px
        width: 22px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
        margin-right: 4px
        vertical-align: top
        margin-top: 8px
      .header_b_m
        font-size: 10px
      .icon-keyboard_arrow_right
        position: absolute
        right: 8px
        top: 8px
        font-size: 10px

    .background
      position: absolute
      width: 100%
      height: 100%
      top: 0
      left: 0
      z-index: -1
      filter: blur(10px)
      img
        width: 100%
        height: 100%
    .detail
      position fixed
      z-index 100
      width: 100%
      height: 100%
      left: 0
      top: 0
      overflow auto
      transition: all 0.5s
      backdrop-filter: blur(10) px
      &.fade-transition
        opacity: 1
        background: rgba(7, 17, 27, 0.8)
      &.fade-enter, &.fade-leave
        opacity: 0
        background: rgba(7, 17, 27, 0)
      .detail-wrapper
        min-height: 100%
        width: 100%
        .detail-main
          margin-top 64px
          padding-bottom 64px
          .name
            font-size 16px
            font-weight 700
            line-height: 16px
            text-align center
          .star-wrapper
            height: 24px
            margin-top 16px
            text-align center
          .title
            width: 80%
            display flex
            margin 28px auto 24px auto
            .line
              flex 1
              position: relative
              top: 6px
              border-top 1px solid rgba(255, 255, 255, 0.2)
            .message
              padding: 0 12px
              font-weight 700
              line-height: 14px
          .supports
            width: 80%
            padding: 0 12px
            margin: 0 auto
            .supports-item
              height: 16px
              margin-bottom 12px
              font-size 0
              &:last-child
                margin-bottom 0
              .supports_l
                display inline-block
                height: 16px
                width: 16px
                vertical-align top
                margin-right 6px
                background-size 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
              .supports_r
                font-size 12px
                font-weight 200
                line-height: 16px
          .bulletin
            width: 80%
            margin 0 auto
            .bulletin-p
              font-size 12px
              line-height: 24px
              padding: 0 12px
      .detail-close
        position: relative
        width 32px
        height: 32px
        margin: -64px auto 0 auto
        clear: both
        font-size: 32px
</style>
