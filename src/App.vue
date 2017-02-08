<template>
  <div id="app">
    <div class="header">
      <v-header :seller="seller"></v-header>
    </div>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link> 
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link> 
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link> 
      </div>
    </div>
    <div class="content">
       <keep-alive>
         <router-view :seller="seller" ></router-view>
       </keep-alive>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import axios from 'axios'
import header from './components/header/header.vue'
const ERR_OK = 0
export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  created () {
    axios.get('/api/seller').then((res) => {
      res = res.data
      if (res.errno === ERR_OK) {
        this.seller = res.data
      }
    })
  },
  components: {
    'v-header': header
  }
}
</script>

<style lang="scss" style="stylesheet/scss">
  @import "common/scss/mixin";
  #app {
    .tab {
      display: flex;
      width: 100%;
      height: pxToRem(40);
      line-height: pxToRem(40);
      -webkit-overflow-scrolling: touch;
      @include border-1px(rgba(7, 17, 27, 0.1));
      .tab-item {
        flex:1;
        text-align:center;
        a {
          display: block;
          text-decoration: none;
          font-size: pxToRem(14);
          color:rgb(77,85,93);
        }
        .router-link-active{
          color: rgb(240,20,20);
        }
      }
    }
  }

</style>
