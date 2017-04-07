<template>
  <div>
    <div class="header">
      <headerOne :seller="seller"></headerOne>
    </div>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from 'components/header/header.vue';

  const ERR_OK = 0;

  export default {
    data() {
      return {
        seller: {
          type: Object
        }
      };
    },
    created() {
      this.$http.get('/api/seller').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = response.data;
        }
      });
    },
    components: {
      headerOne: header
    }
  };
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "./common/scss/mixin.scss";
  .tab{
    width: 100%;
    height: size(80);
    display: flex;
    border-bottom: size(1) solid rgba(7,17,27,0.1);
    .tab-item{
      flex: 1;
      margin:auto;
      text-align: center;
      font-size:size(28);
      line-height: size(28);
      a{
        display: block;
        text-decoration:none;
        color: rgb(77,85,93);
        &.active{
          color: rgb(240,20,20);
        }
      }
    }
  }
</style>
