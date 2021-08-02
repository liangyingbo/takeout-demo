<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <!-- <router-view/> -->
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
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue'
import axios from 'axios'
export default {
  components: { 'v-header':header },
  name: 'App',
  data(){
    return {
      seller:{}
    }
  },
    created() {
    axios.get('static/data.json').then((res) => {
      this.seller = res.data.seller
      // console.log(this.seller)
    })
  },
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import 'common/stylus/index'
  .tab
    display flex
    height 40px
    line-height 40px
    width 100%
    .tab-item
      flex 1
      text-align center
      font-size 14px
      color rgb(77,85,93)
    .router-link-active
      color #f01414
</style>
