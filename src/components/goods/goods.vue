<template>
  <div class="goods">
    <div class="goods_l" v-el:menu-wrapper>
      <ul class="menu_wrapper">
        <li class="menu-item" v-for="item in goods" :class="{'current':currentIndex===$index}"
            @click="selectMenu($index,$event)">
          <span class="text border-1px">
            <span class="icon" v-if="item.type>0" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="goods_r" v-el:food-wrapper>
      <ul>
        <li class="food-list food-list-hook" v-for="item in goods">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li @click="selectFood(food,$event)" class="food-item border-1px" v-for="food in item.foods">
              <div class="goods_pic">
                <img widht="57" height="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extre">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="new">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="catcontrol-wrapper">
                  <catcontrol :food="food"></catcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shortcat v-ref:shortcat :select-foods="selectFoods" :delivery-price="seller.deliveryPrice"
              :min-price="seller.minPrice"></shortcat>
  </div>
  <food :food="selectedFood" v-ref:food></food>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shortcat from '../shortcat/shortcat.vue';
  import catcontrol from '../catcontrol/catcontrol.vue';
  import food from 'components/food/food.vue';
  const ERR_OK = 0;

  export default {
    props: {
      seller: {}
    },
    data() {
      return {
        goods: {
          type: Object
        },
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
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
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();  // 计算高度的方法
          });
        }
      });
    },
    methods: {
      _initScroll() {
        this.meunScroll = new BScroll(this.$els.menuWrapper, {
          click: true
        });
        this.foodScroll = new BScroll(this.$els.foodWrapper, {
          click: true,
          probeType: 3
        });

        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      selectFood(food, event) {
        if (!event._constructed) {
            return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      },
      _calculateHeight() {
        let foodList = this.$els.foodWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index, event) {
        if (!event._constructed) {
          return;
        }
        let foodList = this.$els.foodWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodScroll.scrollToElement(el, 300);
      },
      _drop(target) {
        this.$nextTick(() => {
          this.$refs.shortcat.drop(target);
        });
      }
    },
    components: {
      shortcat,
      catcontrol,
      food
    },
    events: {
      'cart.add'(target) {
        this._drop(target);
      }
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
        padding: 0 12px
        &.current
          position: relative
          margin-top -1px
          z-index 10
          background: #fff
          font-weight 700
          .text
            border-none()
        .text
          display table-cell
          vertical-align middle
          text-align center
          width: 56px
          font-size 12px
          border-1px(rgba(7, 17, 27, 0.1))
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
      .title
        padding-left 14px
        height: 26px
        line-height: 26px
        font-size 12px
        border-left 2px solid #d9dde1
        color: rgb(147, 153, 159)
        background-color: #f3f5f7
      .food-item
        display flex
        border-1px(rgba(7, 17, 27, 0.1))
        margin: 18px
        padding-bottom 18px
        &:last-child
          border-none()
          padding-bottom 0
        .goods_pic
          flex 0 0 57px
          margin-right 10px
        .content
          flex: 1
          .name
            font-size 14px
            color: rgb(7, 17, 27)
            height: 14px
            line-height: 14px
            margin: 2px 0 8px
          .desc
            margin-bottom 8px
            font-size 10px
            line-height: 12px
            color: rgb(147, 153, 159)
          .extre
            font-size 10px
            line-height: 10px
            color: rgb(147, 153, 159)
            .count
              margin-right 12px
          .price
            font-weight 700
            line-height: 24px
            .new
              font-size 14px
              color: rgb(240, 20, 20)
              margin-right 8px
            .old
              font-size 10px
              color: rgb(147, 153, 159)
              text-decoration line-through
          .catcontrol-wrapper
            position: absolute
            right: 0px
            bottom: 5px

</style>
