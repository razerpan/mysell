<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="titles">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent"
                    :ratings="ratings"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)" class="rating-item">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar" alt="">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.delivery"></span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span class="item" v-for="item in rating.recommend">{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from 'components/star/star';
  import split from 'components/split/split';
  import ratingselect from 'components/ratingselect/ratingselect';
  import BScroll from 'better-scroll';
  import {formatDate} from 'common/js/date';
  const ALL = 2;
  const ERR_OK = 0;
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: true
      };
    },
    created() {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.ratings = response.data;
          this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.ratings, {
              click: true
            });
          });
        }
      });
    },
    methods: {
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
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    components: {
      star,
      split,
      ratingselect
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .ratings {
    position: absolute;
    width: 100%;
    top: size(363);
    left: 0;
    bottom: 0;
    overflow: hidden;
    .overview {
      display: flex;
      padding: size(36) 0;
      .overview-left {
        flex: 0 0 size(275);
        padding: size(12) 0;
        width: size(275);
        border-right: size(1) solid rgba(7, 17, 27, 0.1);
        text-align: center;
        .score {
          margin-bottom: size(12);
          line-height: size(56);
          font-size: size(48);
          color: rgb(255, 153, 0);
        }
        .titles {
          width: 100%;
          margin: 0 auto size(16) auto;
          line-height: size(24);
          font-size: size(24);
          color: rgb(7, 17, 27);
        }
        .rank {
          line-height: size(20);
          font-size: size(20);
          color: rgb(147, 153, 159);
        }
      }
      .overview-right {
        flex: 1;
        padding: size(12) 0 size(12) size(48);
        .score-wrapper {
          margin-bottom: size(16);
          font-size: 0;
          .title {
            line-height: size(36);
            display: inline-block;
            font-size: size(24);
            color: rgb(7, 17, 27);
          }
          .star {
            display: inline-block;
            margin: 0 size(24);
            vertical-align: top;
          }
          .score {
            line-height: size(36);
            display: inline-block;
            font-size: size(24);
            color: rgb(255, 153, 0);
          }
        }
        .delivery-wrapper {
          font-size: 0;
          .title {
            line-height: size(36);
            display: inline-block;
            font-size: size(24);
            color: rgb(7, 17, 27);
          }
          .delivery {
            margin-left: size(24);
            line-height: size(36);
            font-size: size(24);
            color: rgb(147, 153, 159);
          }
        }
      }
    }
    .rating-wrapper {
      padding: 0 size(36);
      .rating-item {
        display: flex;
        padding: size(36) 0;
        border-bottom: size(1) solid rgba(7, 17, 27, 0.1);
        .avatar {
          flex: 0 0 size(56);
          width: size(56);
          margin-right: size(24);
          img {
            border-radius: 50%;
          }
        }
        .content {
          position: relative;
          flex: 1;
          .name {
            margin-bottom: size(8);
            line-height: size(24);
            font-size: size(20);
            color: rgb(7, 17, 27);
          }
          .star-wrapper {
            margin-bottom: size(12);
            font-size: 0;
            .star {
              display: inline-block;
              margin-right: size(12);
              vertical-align: top;
            }
            .delivery {
              display: inline-block;
              vertical-align: top;
              line-height: size(24);
              font-size: size(20);
              color: rgb(147, 153, 159);
            }
          }
          .text {
            margin-bottom: size(16);
            line-height: size(36);
            color: rgb(7, 17, 27);
            font-size: size(24);
          }
          .recommend {
            line-height: size(32);
            font-size: 0;
            .icon-thumb_up, .item {
              display: inline-block;
              margin: 0 size(16) size(8) 0;
              font-size: size(18);
            }
            .icon-thumb_up {
              color: rgb(0, 160, 220);
            }
            .item {
              padding: 0 size(12);
              border: size(1) solid rgba(7, 17, 27, 0.1);
              border-radius: size(1);
              color: rgb(147, 153, 159);
              background: #fff;
            }
          }
          .time {
            position: absolute;
            top: 0;
            right: 0;
            line-height: size(24);
            font-size: size(20);
            color: rgb(147, 153, 159);
          }
        }
      }

    }

  }
</style>
