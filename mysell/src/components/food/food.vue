<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol @add="addFood" :food="food"></cartcontrol>
          </div>
          <transition name="fade">
            <div @click="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
          </transition>
        </div>
        <split v-show="food.info"></split>
        <div class="info">
          <h1 class="title" v-show="food.info">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType"
                        :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" alt="" :src="rating.avatar" width="12" height="12">
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <p class="text">
                  <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  import Vue from 'vue';
  import {formatDate} from 'common/js/date';
  import split from 'components/split/split';
  import ratingselect from 'components/ratingselect/ratingselect';

  //  const POSITIVE = 0;
  //  const NEGATIVE = 1;
  const ALL = 2;

  export default{
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      show() {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = false;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      hide() {
        this.showFlag = false;
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        console.log(event.target);
        this.$emit('add', event.target);
        Vue.set(this.food, 'count', 1);
      },
      needShow(type, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
      },
      addFood(target) {
        this.$emit('add', target);
      },
      selectRating(type) {
        this.selectType = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      toggleContent() {
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: size(96);
    z-index: 30;
    width: 100%;
    background: #ffffff;
    transform: translate3d(0, 0, 0);
    &.move-enter-active, &.move-leave-active {
      transition: all 0.2s linear;
    }
    &.move-enter, &.move-leave-active {
      transform: translate3d(100%, 0, 0);
    }
    .image-header {
      position: relative;
      width: 100%;
      height: 0;
      padding-top: 100%;
      img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
      }
      .back {
        position: absolute;
        top: size(20);
        left: 0;
        .icon-arrow_lift {
          display: block;
          padding: size(20);
          font-size: size(40);
          color: rgb(0, 160, 220);
        }
      }
    }
    .content {
      padding: size(36);
      position: relative;
      .title {
        line-height: size(28);
        margin-bottom: size(16);
        font-size: size(28);
        font-weight: 700;
        color: rgb(7, 17, 27);
      }
      .detail {
        margin-bottom: size(36);
        line-height: size(20);
        height: size(20);
        font-size: 0;
        .sell-count, .rating {
          font-size: size(20);
          color: rgb(147, 153, 159);
        }
        .sell-count {
          margin-right: size(24);
        }
      }
      .price {
        font-weight: 700;
        line-height: size(48);
        .now {
          margin-right: size(16);
          font-size: size(28);
          color: rgb(240, 20, 20);
        }
        .old {
          text-decoration: line-through;
          font-size: size(20);
          color: rgb(147, 153, 159);
        }
      }
    }
    .cartcontrol-wrapper {
      position: absolute;
      right: size(24);
      bottom: size(24);
    }
    .buy {
      position: absolute;
      right: size(36);
      bottom: size(36);
      z-index: 10;
      height: size(48);
      line-height: size(48);
      padding: 0 size(24);
      box-sizing: border-box;
      border-radius: size(24);
      font-size: 10px;
      color: #ffffff;
      background: rgb(0, 160, 220);
      opacity: 1;
      &.fade-enter-active, &.fade-leave-active {
        transition: all 0.2s;
      }
      &.fade-enter, &.fade-leave-active {
        opacity: 0;
      }
    }
    .info {
      padding: size(36);
      .title {
        line-height: size(28);
        margin-bottom: size(12);
        font-size: size(28);
        color: rgb(7, 17, 27);
      }
      .text {
        color: rgb(77, 85, 93);
        line-height: size(48);
        padding: 0 size(16);
        font-size: size(24);
      }
    }
    .rating {
      padding-top: size(36);
      .title {
        line-height: size(28);
        margin-left: size(36);
        font-size: size(28);
        color: rgb(7, 17, 27);
      }
      .rating-wrapper {
        padding: 0 size(36);
        .rating-item {
          position: relative;
          padding: size(32) 0;
          border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
          .user {
            position: absolute;
            right: 0;
            top: size(32);
            font-size: 0;
            line-height: size(24);
            .name {
              display: inline-block;
              vertical-align: top;
              font-size: size(20);
              color: rgb(147, 153, 159);
              margin-right: size(12);
            }
            .avatar {
              border-radius: 50%;
            }
          }
          .time {
            margin-bottom: size(12);
            line-height: size(24);
            font-size: size(20);
            color: rgb(147, 153, 159);
          }
          .text {
            line-height: size(32);
            font-size: size(24);
            color: rgb(7, 17, 27);
            .icon-thumb_up {
              margin-right: size(8);
              line-height: size(32);
              font-size: size(24);
              color: rgb(0, 160, 220);
            }
            .icon-thumb_down {
              margin-right: size(8);
              line-height: size(32);
              font-size: size(24);
              color: rgb(147, 153, 159);
            }
          }
        }
        .no-rating {
          padding: size(32) 0;
          font-size: size(24);
          color: rgb(147, 153, 159);
        }
      }
    }
  }
</style>
