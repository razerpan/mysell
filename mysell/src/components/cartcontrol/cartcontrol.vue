<template>
  <div class="cartcontrol">
    <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0"></div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default{
    props: {
      food: {
        type: Object
      }
    },
    created() {
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          this.food.count = 1;
        } else {
          this.food.count++;
        }
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .cartcontrol {
    font-size: 0;
    .cart-decrease, .cart-add {
      display: inline-block;
      padding: size(12);
      font-size: size(48);
      line-height: size(48);
      color: rgb(0, 160, 220);
    }
    .cart-count {
      display: inline-block;
    }
    .cart-add {

    }
  }
</style>
