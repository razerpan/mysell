<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="left-img">
        <img :src="seller.avatar" alt="">
      </div>
      <div class="right-content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}} / {{seller.deliveryTime}}分钟到达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin" @click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrap">
          <div class="detail-main clearfix">
            <h1 class="name">{{seller.name}}</h1>
          </div>
        </div>
        <div class="detail-close">
          <span class="icon-close" @click="hideDetail"></span>
        </div>
      </div>
    </transition>
  </div>
</template>
<script type="text/ecmascript-6">
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      };
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    mounted() {
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    }

  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .header {
    background: rgba(7, 17, 27, 0.3);
    overflow: hidden;
    position: relative;
    .content-wrapper {
      width: 100%;
      display: flex;
      padding: size(48) 0 size(36) size(48);
      .left-img {
        width: size(128);
        margin-right: size(32);
        img {
          width: size(128);
          height: size(128);
          border-radius: size(4);
        }
      }
      .right-content {
        flex: 1;
        .title {
          padding-top: size(4);
          font-size: 0;
          .brand {
            display: inline-block;
            vertical-align: top;
            width: size(60);
            height: size(36);
            @include bg-image('brand');
            background-size: size(60) size(36);
            background-repeat: no-repeat;
          }
          .name {
            margin: size(12);
            color: rgb(255, 255, 255);
            font-size: size(32);
            font-weight: bold;
            line-height: size(36);
          }
        }
        .description {
          padding-top: size(16);
          padding-bottom: size(20);
          font-size: size(24);
          line-height: size(24);
          font-weight: 200;
          color: rgb(255, 255, 255);
        }
        .support {
          font-size: 0;
          .icon {
            display: inline-block;
            vertical-align: top;
            width: size(24);
            height: size(24);
            margin-right: size(8);
            background-size: size(24) size(24);
            background-repeat: no-repeat;
            &.decrease {
              @include bg-image('decrease_1');
            }
            &.discount {
              @include bg-image('decrease_1');
            }
            &.guarantee {
              @include bg-image('decrease_1');
            }
            &.invoice {
              @include bg-image('decrease_1');
            }
            &.special {
              @include bg-image('decrease_1');
            }
          }
          .text {
            font-size: size(20);
            line-height: size(24);
            font-weight: 200;
            color: rgb(255, 255, 255);
          }
        }
      }
      .support-count {
        position: absolute;
        bottom: size(92);
        right: size(28);
        height: size(48);
        padding: 0 size(16);
        line-height: size(48);
        border-radius: size(24);
        background-color: rgba(0, 0, 0, .2);
        text-align: center;
        font-size: 0;
        color: rgb(255, 255, 255);
        .count {
          vertical-align: top;
          font-size: size(20);
        }
        .icon-keyboard_arrow_right {
          font-size: size(20);
          line-height: size(48);
          margin-left: size(4);
        }
      }
    }
    .bulletin {
      position: relative;
      height: size(56);
      line-height: size(56);
      padding-right: size(48);
      background-color: rgba(7, 17, 27, 0.2);
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      color: rgb(255, 255, 255);
      .bulletin-title {
        display: inline-block;
        vertical-align: top;
        width: size(44);
        height: size(24);
        margin: size(16) size(8) size(16) size(24);
        @include bg-image('bulletin');
        background-repeat: no-repeat;
        background-size: size(44) size(24);
      }
      .bulletin-text {
        vertical-align: top;
        margin-top: size(16);
        font-size: size(20);
        font-weight: 200;
        color: rgb(255, 255, 255);
      }
      .icon-keyboard_arrow_right {
        position: absolute;
        right: size(24);
        top: size(16);
        font-size: size(20);
      }
    }
    .background {
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      filter: blur(10px);
    }
    .detail {
      position: fixed;
      z-index: 1;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      /*padding-right: size(17);*/
      background-color: rgba(7, 17, 27, 0.8);
      backdrop-filter: blur(10px);
      &.fade-enter-active, &.fade-leave-active {
        transition: all 1s;
      }
      &.fade-enter, &.fade-leave-active {
        opacity: 0;
        background: rgba(7, 17, 27, 0);
      }
      .detail-wrap {
        width: 100%;
        min-height: 100%;
        .detail-main {
          width: 100%;
          margin-top: size(128);
          padding-bottom: size(128);
          .name{
            text-align: center;
            font-size: size(32);
            line-height: size(32);
            font-weight: 700;
            color: rgb(255,255,255);
          }
        }
      }
      .detail-close {
        position: relative;
        margin-top: size(-128);
        clear: both;
        height: size(128);
        span{
          display: block;
          margin: 0 auto;
          text-align: center;
          font-size: size(64);
          color: rgba(255,255,255,0.5);
        }
      }
    }
  }
</style>
