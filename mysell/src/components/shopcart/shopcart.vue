<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}</div>
      </div>
      <div class="content-right">
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
          return [
            {
              price: 50,
              count: 1
            }
          ];
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
          return `￥${this.minPrice}起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}起送`;
        } else {
          return '去结算';
        }
      },
      payClass() {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .shopcart {
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 50;
    width: 100%;
    height: size(96);
    .content {
      display: flex;
      background: #141d27;
      font-size: 0;
      .content-left {
        flex: 1;
        .logo-wrapper {
          display: inline-block;
          position: relative;
          top: size(-20);
          margin: 0 size(24);
          padding: size(12);
          height: size(112);
          width: size(112);
          box-sizing: border-box;
          vertical-align: top;
          border-radius: 50%;
          background: #141d27;
          .num {
            position: absolute;
            top: 0;
            right: 0;
            width: size(48);
            height: size(32);
            line-height: size(32);
            text-align: center;
            border-radius: size(32);
            font-size: size(18);
            font-weight: 700;
            color: #ffffff;
            background: rgb(240, 20, 20);
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
          }
          .logo {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: #2b343c;
            text-align: center;
            &.highlight {
              background: rgb(0, 160, 220);
            }
            .icon-shopping_cart {
              font-size: size(48);
              line-height: size(88);
              color: #80858a;
              &.highlight {
                color: #ffffff;
              }
            }
          }
        }
        .price {
          display: inline-block;
          vertical-align: top;
          line-height: size(48);
          margin-top: size(24);
          padding-right: size(24);
          box-sizing: border-box;
          border-right: size(1) solid rgba(255, 255, 255, 0.1);
          font-size: size(32);
          font-weight: 700;
          color: rgba(255, 255, 255, 0.4);
          &.highlight {
            color: #fff;
          }
        }
        .desc {
          display: inline-block;
          vertical-align: top;
          line-height: size(48);
          margin: size(24) 0 0 size(24);
          font-size: size(20);
          color: rgba(255, 255, 255, 0.4);
        }
      }
      .content-right {
        flex: 0 0 size(210);
        width: size(210);
        .pay {
          height: size(96);
          line-height: size(96);
          text-align: center;
          font-size: size(24);
          color: rgba(255, 255, 255, 0.4);
          font-weight: 700;
          &.not-enough {
            background: #2b333b;
          }
          &.enough {
            background: #00b43c;
            color: #ffffff;
          }
        }
      }
    }
  }
</style>
