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
          <span>{{seller.description}}</span>/<span>{{seller.deliveryTime}}</span>分钟送达
        </div>
        <div class="supports" v-if="seller.supports">
          <span></span>
          <span>{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="header_b">

    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      seller: {}
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .header
    background: #000
    color: rgb(255, 255, 255)
    .header_t
      padding: 24px 12px 18px 24px
      font-size: 0 // 这里font-size 设置为0 不让子元素继承自己的字体大小
      .avatar
        display: inline-block
        img
          border-radius:2px
      .content
        display inline-block
        vertical-align: top
        font-size: 14px // 这里单独设置字体大小 让左右两边元素贴在一起
        margin-left: 16px
        .title
          margin: 2px 0 8px 0
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
            font-weight:bold

</style>
