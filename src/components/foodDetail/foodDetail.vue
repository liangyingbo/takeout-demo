 <template>
  <transition name="move">
    <div class="detailWrapper" ref="detailWrapper" v-show="showDetail">
      <div class="foodDetail">
        <div class="back" @click="showToggle()">
          <i class="icon-arrow_lift"></i>
        </div>
        <img :src="food.image" width="100%" height="400" alt="" />

        <div class="info">
          <div class="title">{{ food.name }}</div>
          <div class="desc">
            <span>月售{{ food.sellCount }}</span>
            <span>好评率{{ food.rating }}%</span>
          </div>
          <div class="price">
            <span>￥{{ food.price }}</span>
            <span v-if="food.oldPrice">￥{{ food.oldPrice }}</span>
          </div>
          <div class="shopCart">
            <span @click="addCart($event)" v-show="!food.count">加入购物车</span>
          </div>
          <cartcontrol :food='food'></cartcontrol>
        </div>
        <div class="divider"></div>
        <div class="desc1">
          <div class="title">商品介绍</div>
          <div class="content">{{ food.info }}</div>
        </div>
        <div class="divider"></div>
        <div class="evaluation">
          <div class="title">商品评价</div>
           <div class="classify">
               <span v-for="(item,index) in classifyArr" class="item" :class="{'active':item.active,'bad':index==2,'badActive':item.active&&index==2}" @click="filterEvel(item)">
                   {{item.name}}<span class="count">{{item.count}}</span>
               </span>
           </div>
        </div>
      </div>
    </div>
  </transition>
</template>
  
<script>
import BScroll from "better-scroll";
import cartcontrol from '@/components/cartcontrol/cartcontrol'
export default {
  props: {
    food: {
      type: Object,
      default: {},
    },
  },
  data() {
    return {
      showDetail: false,

      classifyArr: [
        {
          name: "全部",
          count: this.food.ratings.length,
          active: true,
        },
        {
          name: "推荐",
          count: this.food.ratings.filter(x => x.rateType ===0).length,
          active: false,
        },
        {
          name: "吐槽",
          count: this.food.ratings.filter( x => x.rateType ===1).length,
          active: false,
        },
      ],
    };
  },
  create() {},
  mounted() {},
  computed: {
    //   classifyArr(){
    //       let arr = []
    //       let ss = {}
    //       ss.name = '全部'
    //       ss.count = this.food.ratings.length
    //       console.log(this.food.ratings.length)
    //       ss.active = true
    //       arr.push(ss)
    //       return arr;
    //   }
  },
  methods: {
    showToggle() {
      this.showDetail = !this.showDetail;
      if (this.showDetail) {
        this.$nextTick(() => {
          this._initScroll();
        });
      }
    },
    _initScroll() {
      if (!this.detailWrapper) {
        this.detailWrapper = new BScroll(this.$refs.detailWrapper, {
          click: true,
        });
      } else {
        this.detailWrapper.refresh();
      }
    },
    filterEvel(item){
        this.classifyArr.forEach(
           (data) => { data.active = false } 
        )
        item.active = true 
    },
    addCart(e){
       if(!e._constructed){
         return 
       }
       this.$set(this.food,'count',1)
       this.$root.eventHub.$emit('cart_add',e.target)
    }
  },
  components:{
    cartcontrol
  }
};
</script>
<style lang="stylus" scoped>
.detailWrapper {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  bottom: 48px;
  background-color: #fff;
  transition: all 0.4s ease;
  overflow: hidden;
  z-index 50
  &.move-enter-active, &move-leave-active {
    transform: translate3d(0, 0, 0);
  }

  &.move-enter, &.move-leave-active {
    transform: translate3d(100%, 0, 0);
  }

  .foodDetail {
    .back {
      top: 12px;
      position: absolute;
      left: 10px;
      font-size: 16px;
      color: #fff;
    }

    .info {
      position: relative;
      // background-color pink
      padding: 18px;

      .shopCart {
        position: absolute;
        right: 18px;
        bottom: 18px;
        background-color: #00a0dc;
        border-radius: 15px;
        line-height: 22px;
        color: #fff;
        z-index 21
        span {
          padding: 8px;
          font-size: 12px;
        }
        
        
      }

      .title {
        font-size: 14px;
        font-weight: bold;
        margin: 8px 0px;
      }

      .desc {
        font-size: 12px;
        margin-bottom: 15px;

        span:first-child {
          padding-right: 10px;
        }
      }

      .price {
        color: #f01414;
        font-size: 14px;

        .price span {
          font-weight: bold;
        }
      }

      .cartcontrol{
          position absolute
          right 18px
          bottom 18px
        }
    }

    .divider {
      width: 100%;
      height: 15px;
      background-color: #f3f5f7;
      border-top: 1px solid rgba(7, 17, 27, 0.1);
      border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }

    .desc1 {
      padding: 18px;

      .title {
        font-size: 14px;
        font-weight: 500;
        padding-bottom: 5px;
      }

      .content {
        font-size: 12px;
        padding: 0px 8px;
        color: #4d555d;
        line-height: 24px;
      }
    }

    .evaluation {
        padding 18px 0px 
        .title{
            padding-left 18px
            font-size 15px
            color #07111b
        }
        .classify{
             padding 18px 0px
             margin 0px 18px
             border-bottom 1px solid rgba(7,17,27,0.1)
        }
            .item{
                font-size: 14px;
                // background-color: burlywood;
                display: inline-block;
                margin-right: 6px;
                padding: 8px 12px;

                &.active{
                    color white
                    background rgb(0,169,220)
                }
                    
                &.bad{
                    background rgba(77,85,93,0.2)
                }
                     
                &.badActive{
                     background #4d555d
                }

            }
                .count{
                     padding-left 2px
                }
               
               
                   
           
    }
  }
}
</style>
