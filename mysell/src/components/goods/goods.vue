<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}"
            @click="selectMenu(index,$event)">
          <span class="text"><span v-show="item.type>0" class="icon"
                                   :class="classMap[item.type]"></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  const ERR_OK = 0;

  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
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
            this._calculateHeight();
          });
        }
      });
    },
    methods: {
      _initScroll() {
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight() {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
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
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
        console.log(index);
      }
    },
    components: {
      shopcart,
      cartcontrol
    }

  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .goods {
    display: flex;
    position: absolute;
    width: 100%;
    top: size(363);
    bottom: size(92);
    overflow: hidden;
    .menu-wrapper {
      flex: 0 0 size(160);
      width: size(160);
      background-color: #f3f5f7;
      .menu-item {
        display: table;
        height: size(108);
        padding: 0 size(24);
        line-height: size(28);
        /*margin: 0 auto;*/
        &.current {
          position: relative;
          margin-top: size(-1);
          z-index: 10;
          background: #fff;
          font-weight: 700;
          .text {
            border-bottom: none;
          }
        }
        .icon {
          display: inline-block;
          vertical-align: top;
          width: size(24);
          height: size(24);
          margin-right: size(4);
          background-size: size(24) size(24);
          background-repeat: no-repeat;
          &.decrease {
            @include bg-image('decrease_3');
          }
          &.discount {
            @include bg-image('discount_3');
          }
          &.guarantee {
            @include bg-image('guarantee_3');
          }
          &.invoice {
            @include bg-image('invoice_3');
          }
          &.special {
            @include bg-image('special_3');
          }
        }
        .text {
          display: table-cell;
          vertical-align: middle;
          width: size(112);
          font-size: size(24);
          border-bottom: size(1) solid rgba(7, 17, 27, 0.2);
        }
      }
    }
    .foods-wrapper {
      flex: 1;
      .title {
        padding-left: size(28);
        height: size(52);
        line-height: size(52);
        border-left: size(8) solid #d9dde1;
        font-size: size(24);
        color: rgb(147, 153, 159);
        background: #f3f5f7;
      }
      .food-item {
        display: flex;
        margin: size(36);
        padding-bottom: size(36);
        border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
        &:last-child {
          border-bottom: none;
          margin-bottom: 0;
        }
        .icon {
          flex: 0 0 size(114);
          margin-right: size(20);
          img {
            width: size(114);
            height: size(114);
          }
        }
        .content {
          flex: 1;
          position: relative;
          .name {
            margin: size(4) 0 size(16) 0;
            height: size(28);
            line-height: size(28);
            font-size: size(28);
            color: rgb(7, 17, 27);
          }
          .desc, .extra {
            line-height: size(20);
            font-size: size(20);
            color: rgb(147, 153, 159);
          }
          .desc {
            margin-bottom: size(16);
            line-height: size(30);
          }
          .extra {
            &.count {
              margin-right: size(24);
            }
          }
          .price {
            line-height: size(48);
            .now {
              font-size: size(28);
              font-weight: 700;
              color: rgb(240, 20, 20);
            }
            .old {
              text-decoration: line-through;
              font-size: size(20);
              font-weight: normal;
              color: rgb(147, 153, 159);
            }
          }
          .cartcontrol-wrapper {
            position: absolute;
            right: 0;
            bottom: size(24);
          }
        }
      }
    }
  }
</style>
