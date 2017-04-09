<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue';

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
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        this.$emit('add', event.target);
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .cartcontrol {
    font-size: 0;
    .cart-decrease {
      display: inline-block;
      padding: size(12);
      opacity: 1;
      transform: translate3d(0, 0, 0);
      .inner {
        display: inline-block;
        font-size: size(48);
        line-height: size(48);
        color: rgb(0, 160, 220);
        transition: all 0.4s linear;
        transform: rotate(0deg);
      }
      &.move-enter-active, &.move-leave-active {
        transition: all 0.4s linear;
      }
      &.move-enter, &.move-leave-active {
        opacity: 0;
        -webkit-transform: translate3d(size(48), 0, 0);
        -moz-transform: translate3d(size(48), 0, 0);
        -ms-transform: translate3d(size(48), 0, 0);
        -o-transform: translate3d(size(48), 0, 0);
        transform: translate3d(size(48), 0, 0);
        .inner {
          transform: rotate(360deg);
        }
      }
    }
    .cart-count {
      display: inline-block;
      vertical-align: top;
      width: size(24);
      padding-top: size(12);
      line-height: size(48);
      text-align: center;
      font-size: size(20);
      color: rgb(147, 153, 159);
    }
    .cart-add {
      display: inline-block;
      padding: size(12);
      font-size: size(48);
      line-height: size(48);
      color: rgb(0, 160, 220);
    }
  }
</style>
