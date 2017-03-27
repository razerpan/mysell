<template>
  <div class="title">
    <div class="line"></div>
    <div class="text">{{content}}</div>
    <div class="line"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default{
    props: {
      text: {
        type: Number
      }
    },
    created() {
      this.titleMap = ['优惠信息', '商家公告'];
    },
    computed: {
      content() {
        return this.titleMap[this.text];
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .title {
    display: flex;
    .line {
      flex: 1;
      position: relative;
      top: size(-12);
      border-bottom: size(1) solid rgba(255, 255, 255, 0.2);
    }
    .text {
      padding: 0 size(24);
      font-size: size(28);
      font-weight: 700;
      color: rgb(255, 255, 255);
    }
  }
</style>
