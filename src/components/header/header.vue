<template>
  <div class="header">
      <div class='content-wrapper'>
          <div class="avatar">
              <img :src="seller.avatar" width="64" height="64" alt="">
          </div>
          <div class="content">
              <div class="title">
                  <span class="brand"></span>
                  <span class="name">{{seller.name}}</span>
              </div>
              <div class="description">
                  {{seller.description + '/' + seller.deliveryTime + '分钟送达'}}
              </div>
              <div class="supports" v-if="seller.supports">
                  <div class="suppert_desc">
                      <span class="icon" :class="iconClassMap[seller.supports[0].type]"></span>
                      <span class='text'>{{seller.supports[0].description}}</span>
                  </div>
              </div>
          </div>
          <div class="support-count"  v-if="seller.supports" @click="showDetailClick">
              <span class="count">{{seller.supports.length + '个'}}</span>
              <i class="icon-keyboard_arrow_right"></i>
          </div>
      </div>
      <div class="bulletin-wrapper" @click="showDetailClick">
          <span class="bulletin-title"></span>
          <span class="bulletin-text">{{seller.bulletin}}</span>
          <i class="icon-keyboard_arrow_right"></i>
      </div>
    <div class="background">
      <img :src="seller.avatar"  width="100%" height="100%" />
    </div>
    <transition name="fade">
        <div v-if='showDetail' class="detail">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                    <h1 class="name">{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <star :size="48" :score='seller.score'></star>
                    </div>
                    <div class="title">
                      <div class="line"></div>
                      <div class="text">优惠信息</div>
                      <div class="line"></div>
                    </div>
                  <ul class="supports">
                    <li class="supports-item" v-for="item in seller.supports">
                      <span class="icon" :class='iconClassMap[item.type]'></span>
                      <span class="text">{{item.description}}</span>
                    </li>
                  </ul>
                   <div class="title">
                    <div class="line"> </div>
                    <div class="text">商家公告</div>
                   <div class="line"></div>
                  </div>
                  <div class="bulletin">{{seller.bulletin}}</div>
         
                </div>
            </div>
             <div class="detail-close">
                <i class="icon-close" @click="hideDetail()"></i>
             </div>
        </div>
    </transition>
  </div>
</template>
<script>
import star from '../star/star.vue'
export default {
    props:{
        seller:{
            type: Object
        }
    },
    data(){
        return {
            showDetail:false
        }
    },
    created(){
     this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
     
    },
    methods:{
        showDetailClick(){
            this.showDetail = true;
        },
        hideDetail(){
         
          this.showDetail = false;

        }
     
    },
    components:{
        star
    }
};
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';
.header 
  position: relative;
  background: rgba(7, 17, 27, 0.5);
  color: #fff;

  .content-wrapper
    position:relative
    padding:24px 12px
    display flex
    .avatar
      img
        border-radius 2px
    .content
      margin-left:10px;
      .title
        .brand
          width 30px
          height 18px
          bg-image('brand')
          background-repeat no-repeat
          display inline-block
          background-size 30px 18px
          vertical-align top
        .name
          height 18px
          line-height 18px
          margin-left 5px
          font-weight bold
      .description
        font-size 13px
        margin 8px 0px
      .supports
        .icon
          display inline-block
          vertical-align bottom
          width 12px
          height 12px
          margin-right 5px
          background-repeat no-repeat
          background-size 12px 12px
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.guarantee
            bg-image('guarantee_1')
          &.invoice
            bg-image('invoice_1')
          &.special
            bg-image('special_1')
        .text
          line-height 12px
          font-size 10px
  .support-count
    position absolute
    right 25px
    bottom 25px
    border-radius 15px
    height: 22px;
    line-height 16px
    background-color: rgba(0,0,0,0.2);
    font-size: 12px;
    text-align: center;
    padding 2px 9px
    .count
      font-size 10px
    .icon-keyboard_arrow_right
      font-size 10px
      margin-left 3px  
      line-height 22px  
  .bulletin-wrapper
    position:relative
    height 28px
    line-height 28px;
    padding 0px 10px 0px 12px
    overflow hidden
    text-overflow ellipsis
    white-space nowrap
    .bulletin-title
      bg-image('bulletin')
      width 20px
      height: 12px;
      display: inline-block;
      background-repeat: no-repeat;
      background-size: 100% 100%;
    .bulletin-text
      font-size 12px
      margin 0 10px 0 5px
    .icon-keyboard_arrow_right
      font-size 12px
      position absolute
      right 7px
      bottom 6px
  .background 
    position absolute
    top 0
    left 0
    z-index -1
    width 100%
    height 100%
    filter blur(10px)
  .detail
    position fixed
    top 0px
    left 0px
    height 100%
    width 100%
    background rgba(7,17,27,0.8)   
    z-index 51
    backdrop-filter: blur(8px);
    .detail-wrapper
      min-height 100%
      width 100%
      .detail-main
        // margin-top 66px
        // margin-left 50%
        margin-top 66px
        text-align center
        .name
          font-size 16px
          font-weight bold
          margin-bottom 17px
        .star-wrapper
          margin-bottom 26px
        .title
          display flex
          width 80%
          margin 0 auto
          .line
            flex 1
            height 1px
            background-color #ccc
            display inline-block
            margin auto 0
          .text
            padding 0px 15px
            font-size 14px
            font-weight bold
        .supports
          margin 15px 0px
          font-size 12px
          text-align left
          .supports-item
              margin-top 19px
              padding 0 45px
              .icon
                background-size 100% 100%
                background-repeat no-repeat
                width 16px
                height 16px
                display inline-block
                margin-right 6px
                vertical-align top 
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
        .bulletin
          padding 20px 48px
          font-size 12px
          font-weight 200
          color rgb(255,255,255)
          line-height 24px
          text-align left
     
  .detail-close
      position relative
      width 32px
      height 32px
      margin -64px auto 0 auto
      clear both
      font-size 32px
      color rgba(255,255,255,0.5)
  &.fade-enter-active, &.fade-leave-active {
      transition: opacity 10.5s
    }
  &.fade-enter, &.fade-leave-active {
      opacity: 10.5s
    }
        
</style>