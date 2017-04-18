<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item" v-for="(item,index) in seller.supports">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90" alt="">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from 'components/star/star';
  import split from 'components/split/split';
  import BScroll from 'better-scroll';
  import {saveToLocal, loadFromLocal} from 'common/js/store';

  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        favorite: (() => {
          return loadFromLocal(this.seller.id, 'favorite', false);
        })()
      };
    },
    computed: {
      favoriteText() {
        return this.favorite ? '已收藏' : '收藏';
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    watch: {
      'seller'() {
        this.$nextTick(() => {
          this._initScroll();
          this._initPics();
        });
      }
    },
    mounted() {
      this.$nextTick(() => {
        this._initScroll();
        this._initPics();
      });
    },
    methods: {
      toggleFavorite(event) {
        if (!event._constructed) {
          return;
        }
        this.favorite = !this.favorite;
        saveToLocal(this.seller.id, 'favorite', this.favorite);
        console.log(loadFromLocal(this.seller.id, 'favorite', false));
      },
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      },
      _initPics() {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth + margin) * this.seller.pics.length - margin;
          this.$refs.picList.style.width = width + 'px';
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true, // 横向滚动
                eventPassthrough: 'vertical'
              });
            } else {
              this.picScroll.refresh();
            }
          });
        }
      }
    },
    components: {
      star,
      split
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .seller {
    position: absolute;
    width: 100%;
    top: size(363);
    left: 0;
    bottom: 0;
    overflow: hidden;
    .overview {
      position: relative;
      padding: size(36);
      .title {
        margin-bottom: size(16);
        line-height: size(28);
        font-size: size(28);
        color: rgb(7, 17, 27);
      }
      .desc {
        padding-bottom: size(36);
        border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
        font-size: 0;
        .star {
          display: inline-block;
          margin-right: size(16);
          vertical-align: top;
        }
        .text {
          display: inline-block;
          vertical-align: top;
          margin-right: size(24);
          line-height: size(36);
          font-size: size(20);
          color: rgb(77, 85, 93);
        }
      }
      .remark {
        display: flex;
        padding-top: size(36);
        .block {
          flex: 1;
          text-align: center;
          border-right: 1px solid rgba(7, 17, 27, 0.1);
          &:last-child {
            border: none;
          }
          h2 {
            margin-bottom: size(8);
            line-height: size(20);
            font-size: size(20);
            color: rgb(147, 153, 159);
          }
          .content {
            line-height: size(48);
            font-size: size(20);
            color: rgb(7, 17, 27);
            .stress {
              font-size: size(48);
            }
          }
        }
      }
      .favorite {
        position: absolute;
        right: size(22);
        top: size(36);
        text-align: center;
        width: size(100);
        .icon-favorite {
          display: block;
          margin-bottom: size(8);
          line-height: size(48);
          font-size: size(48);
          color: #d4d6d9;
          &.active {
            color: rgb(240, 20, 20);
          }
        }
        .text {
          line-height: size(20);
          font-size: size(20);
          color: rgb(77, 85, 93);
        }
      }
    }
    .bulletin {
      padding: size(36) size(36) 0 size(36);
      .title {
        margin-bottom: size(16);
        line-height: size(28);
        font-size: size(28);
        color: rgb(7, 17, 27);
      }
      .content-wrapper {
        padding: 0 size(24) size(32) size(24);
        border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
        .content {
          line-height: size(48);
          font-size: size(24);
          color: rgb(240, 20, 20);
        }
      }
      .supports {
        .support-item {
          padding: size(32) size(24);
          font-size: 0;
          border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
          &:last-child {
            border: none;
          }
          .icon {
            display: inline-block;
            width: size(32);
            height: size(32);
            vertical-align: top;
            margin-right: size(12);
            background-size: size(32) size(32);
            background-repeat: no-repeat;
            &.decrease {
              @include bg-image('decrease_4');
            }
            &.discount {
              @include bg-image('discount_4');
            }
            &.guarantee {
              @include bg-image('guarantee_4');
            }
            &.invoice {
              @include bg-image('invoice_4');
            }
            &.special {
              @include bg-image('special_4');
            }
          }
          .text {
            line-height: size(32);
            font-size: size(24);
            color: rgb(7, 17, 27);
          }
        }
      }
    }
    .pics {
      padding: size(36);
      .title {
        margin-bottom: size(24);
        line-height: size(28);
        font-size: size(28);
        color: rgb(7, 17, 27);
      }
      .pic-wrapper {
        width: 100%;
        overflow: hidden;
        white-space: nowrap; //超过屏幕宽度不折行
        .pic-list {
          font-size: 0;
          .pic-item {
            display: inline-block;
            margin-right: size(12);
            /*width: size(240);*/
            /*height: size(180);*/
            &:last-child {
              margin: 0;
            }
          }
        }
      }
    }
    .info {
      padding: size(36) size(36) 0 size(36);
      color: rgb(7, 17, 27);
      .title {
        padding-bottom: size(24);
        border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
        font-size: size(28);
      }
      .info-item {
        padding: size(32) size(24);
        line-height: size(32);
        border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
        font-size: size(24);
        &:last-child {
          border: none;
        }
      }
    }
  }

</style>
