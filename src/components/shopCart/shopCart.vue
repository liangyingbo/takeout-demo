<template>
  <div class="shopCart">
    <div class="content">
      <div class="content-left" @click="listToggle" >
        <div class="logo-wrapper">
          <div class="badge" v-show="totalCount>0">{{totalCount}}</div>
          <div class="logo" :class="{'active': totalPrice > 0}">
            <i class="icon-shopping_cart"></i>
          </div>
        </div>
        <div class="price" :class="{'active': totalPrice > 0}">￥{{totalPrice}}</div>
        <div class="desc">另需要配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="{'enough': minPrice <= totalPrice}">{{CartDesc}}</div>
    </div>
    <transition name='transHeight'>
      <div class="shopcart-list" v-show='listShow'>
        <div class="list-heard">
          <h1 class="title">购物车</h1>
          <span class="empty" @click="empty">清空</span>
        </div>
        <div class="list-content" ref='foodlist'>
          <ul>
            <li v-for="food in selectFoods" class="food">
              <span class="foodname">{{food.name}}</span>
              <span class="foodprice">￥{{food.price * food.count}}</span>
               <div class="cartcontrol-wrapper">
                  <cartcontrol :food='food'></cartcontrol>
               </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
    <div class="backdrop" v-show='showbackdrop' @click="hideBackdrop"></div>
  </div>
</template>
<script>
import cartcontrol from '@/components/cartcontrol/cartcontrol'
import backdrop from '@/components/backdrop/backdrop'
import BScroll from 'better-scroll'
export default {
  components:{
    cartcontrol,
    backdrop
  },
    props:{
        minPrice:Number,
        selectFoods:Array,
        deliveryPrice:Number
    },
  data() {
    return {
      listShow:false
    };
  },
  created() {
   
  },
  mounted() {
    
  },
  computed:{
      totalCount(){
          let count = 0;
          this.selectFoods.forEach(
               x =>{
                   if(x.count && x.count >0){
                       count += x.count
                           console.log(x)
                   }
               }
          )
        return count
    
      },
    totalPrice(){
        let price = 0
        this.selectFoods.forEach(
            x => {
                if(x.count && x.count > 0){
                   price += x.count * x.price
                }
            }
        )
        return price
    },
    CartDesc(){
     let differ = this.minPrice - this.totalPrice
     if(this.totalPrice > 0){
         if(differ >0){
             return `还差￥${differ}元`
         } 
         return "去结算"
     }else{
         return `￥${this.minPrice}起送`
     }
    },
    showbackdrop(){
      if(this.listShow && this.selectFoods.length > 0){
        return true 
      }
      this.listShow = false;
      return false
    }
  },
  methods: {
  _initScroll() {
      this.foodlistScroll = new BScroll(this.$refs.foodlist, {
        click: true
      });
    },
    listToggle() {
      // debugger
      if (!this.selectFoods.length) {
        return
      }
      this.listShow = !this.listShow
      // // this.foodlistScroll.refresh()
      // this.$nextTick(() =>{
      //     this._initScroll()
      // this.foodlistScroll.refresh()
      // console.log(this.foodlistScroll)
      // })
    
      if (this.listShow) {
        this.$nextTick(() => {
          if (!this.foodlistScroll) {
            this._initScroll()
          } else {
            this.foodlistScroll.refresh()
          }
        })
      }
    },
    hideBackdrop(){
      this.listShow = false
    },
    empty(){
      this.$emit('empty')
    }
  },
};
</script>
<style lang="stylus" scoped>
.shopCart {
  position: fixed;
  bottom: 0px;
  left: 0px;
  height: 48px;
  // background-color red
  width: 100%;
  z-index 50
  .content {
    display: flex;
    background: #141d27;
    // height: 100%;
    .content-left {
      flex: 1;

      // background-color pink
      .logo-wrapper {
        display: inline-block;
        position: relative;
        // background-color black
        width: 56px;
        height: 56px;
        margin: 0px 12px;
        top: -10px;
        background: #141d27;
        border-radius: 50%;
        vertical-align top
        box-sizing: border-box
        text-align center
        padding 6px
        .badge {
          width: 24px;
          height: 15px;
          background-color: #f00;
          color: #fff;
          position: absolute;
          right: 0px;
          /* z-index: 9999; */
          font-size: 10px;
          text-align: center;
          line-height: 16px;
          font-weight: bold;
          border-radius: 13px;
          top: 3px;
        }
        .logo{
            width 100%
            height 100%
            // background-color pink
            color: #80858a;
            background: #2b343c;
            border-radius 50%
            font-size 23px
            line-height 45px
            font-weight 700
            &.active{     
                background: #00a0dc;
                color: #fff;
    
        }
        }
      }

      .price {
        display: inline-block;
        position: relative;
        // background-color yellow
        line-height: 24px;
        margin-top: 12px;
        font-size: 17px;
        font-weight: bold;
        border-right: 1px solid rgba(255, 255, 255, 0.1);
        padding-right: 12px;
        color: rgba(255,255,255,0.4);
        vertical-align top
        &.active{
            color:#fff
        }
      }
     
      .desc {
        display: inline-block;
        position: relative;
        // background-color green
        font-size: 10px;
        font-weight 700
        line-height: 24px;
        vertical-align top
        color: rgba(255, 255, 255, 0.4);
        line-height 48px
        margin-left 8px
        
      }
    }

    .content-right {
      background: #2b343c;
      flex: 0 0 95px;
      line-height: 48px;
      text-align: center;
      font-size: 13px;
      font-weight: 700;
      color gray
      &.enough{
            background-color: #00b43c;
            color: #FFF;
        }
    }
  }

  // .shopcart-list{
  //   position fixed
  //   bottom 48px
  //   background-color #fff
  //   width 100%
  //   transform translate3d(0,-100%,0)
  //   z-index -1
  //   &.transHeight-enter-active,&.transHeight-leave-active{
      
  //     transition all 0.5s
  //   }
  //   &.transHeight-enter,&.transHeight-leave-active{

  //     transform translate3d(0,0,0)
  //   }
    // z-index -1
    // transform: translate3d(0,-100%,0);
    // &.transHeight-enter-active,&.transHeight-leave-to{
    //   transition: all .4s ease;
    // }
    // &.transHeight-enter, &.transHeight-leave-active{
    //   transform: translate3d(0,0,0);
    //   // opacity: 0;
    //  }
    .shopcart-list{

    position absolute
    top 0
    left 0
    width 100%
    background white
    transform translate3d(0,-100%,0)
    z-index -1
    &.transHeight-enter-active,&.transHeight-leave-active{

      transition all 0.5s
    }
    &.transHeight-enter,&.transHeight-leave-active{

      transform translate3d(0,0,0)
    }
    .list-heard{
      position relative
      height 45px
      width 100%
      border-bottom: 1px solid rgba(7,17,27,0.1)
      background: #f3f5f7;
      .title{
        display inline-block
        color: #07111b;
        font-size 15px
        line-height 45px
        margin-left 18px
      }
      .empty{
        position absolute
        right 18px
        font-size  15px
        line-height 45px
        color: #00a0dc
      }
    }
    .list-content{
      position relative
      margin 0px 18px
      max-height 150px
      overflow hidden
      .food{
        display flex
        height 48px
        border-bottom: 1px solid rgba(7,17,27,0.1);
        .foodname{
          font-size 14px
          font-weight 700
          line-height 48px
          color: #07111b
          flex 1
        }
        .foodprice{
          line-height 48px
          padding 0px 18px
          color red
        }
        .cartcontrol-wrapper{
          font-size 14px
          margin-top 13px
        }
      }
    }
  }
  .backdrop{
    position fixed
    left 0px
    top  0px
    width 100%
    height 100%
    background-color rgba(7,17,27,0.6)
    backdrop-filter blur(10px)
    z-index -2
  }
}
</style>