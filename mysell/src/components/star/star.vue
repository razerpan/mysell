<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>
<script type="text/ecmascript-6">
  const LENGTH = 5;
  const CLS_ON = 'on';
  const CLS_HALF = 'half';
  const CLS_OFF = 'off';

  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType() {
        return 'star-' + this.size;
      },
      itemClasses() {
        let result = [];
        let score = Math.floor(this.score * 2) / 2;
        let hasDecimal = score % 1 !== 0;
        let integer = Math.floor(score);
        for (let i = 0; i < integer; i++) {
          result.push(CLS_ON);
        }
        if (hasDecimal) {
          result.push(CLS_HALF);
        }
        while (result.length < LENGTH) {
          result.push(CLS_OFF);
        }
        return result;
      }
    }
  };

</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .star {
    .star-item {
      display: inline-block;
      background-repeat: no-repeat;
    }
    &.star-48 {
      .star-item {
        width: size(40);
        height: size(40);
        margin-right: size(44);
        background-size: size(40) size(40);
        &:last-child {
          margin-right: 0;
        }
        &.on {
          @include bg-image('star48_on')
        }
        &.half {
          @include bg-image('star48_half')
        }
        &.off {
          @include bg-image('star48_off')
        }
      }
    }
    &.star-36 {
      .star-item {
        width: size(30);
        height: size(30);
        margin-right: size(12);
        background-size: size(30) size(30);
        &:last-child {
          margin-right: 0;
        }
        &.on {
          @include bg-image('star36_on')
        }
        &.half {
          @include bg-image('star36_half')
        }
        &.off {
          @include bg-image('star36_off')
        }
      }
    }
    &.star-24 {
      .star-item {
        width: size(20);
        height: size(20);
        margin-right: size(6);
        background-size: size(20) size(20);
        &:last-child {
          margin-right: 0;
        }
        &.on {
          @include bg-image('star24_on')
        }
        &.half {
          @include bg-image('star24_half')
        }
        &.off {
          @include bg-image('star24_off')
        }
      }
    }
  }
</style>
