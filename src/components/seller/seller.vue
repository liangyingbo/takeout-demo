<template>
  <div class="sellerWrapper" ref="sellerWrapper">
    <div class="content">
      <div class="info">
          <div style="border-bottom:1px solid #ccc">
               <div class="title">粥品香坊（回龙观）</div>
          <div style="margin-top:8px">
              <div class="star-container"><star :size='36' :score='4'></star></div>
              <span style="font-size:12px;color:#4d555d">(24)</span>
              <span style="font-size:12px;color:#4d555d">月售90单</span>
          </div>
          <div class="favorite">
              <span class="icon-favorite"></span>
              <span>收藏</span>
          </div>
          </div>
         
            <div class="remark">
          <div class="remark-item">
              <h2>起送价</h2>
              <span class="num">20</span>
              <span class="yuan">元</span>
          </div>
          <div class="remark-item">
              <h2>商家配送</h2>
              <span class="num">4</span>
              <span class="yuan">元</span>
          </div>
          <div class="remark-item">
              <h2>平均配送时间</h2>
              <span class="num">38</span>
              <span class="yuan">分钟</span>
         </div>
      </div>
      </div>
    <div class="divider"></div>
    <div class="activities">
        <div class="bulletin">
            <h2>公告与活动</h2>
            <div class="content">
                {{seller.bulletin}}        
            </div>
        </div>
    </div>
    <div class="supports">
        <ul>
            <li v-for="item in seller.supports">
                <icon-map :iconType='item.type'></icon-map>
                <span>{{item.description}}</span>
            </li>
        </ul>
    </div>
    <div class="divider"></div>
    <div class="seller-img">
        <h2>商家实景</h2>
        <div class="content" ref="imgcontainer">
            <div :style="{width: calcWidth + 'px'}">
                <!-- <div style="display:flex;width:500px"> -->
                 <img :src="item" alt="" v-for="item in seller.pics">
            </div>
           
        </div>
    </div>
    <div class="divider"></div>
         <div class="seller-info">
           <h2>商家信息</h2>
           <ul>
             <li v-for="item in seller.infos">{{item}}</li>
           </ul>
         </div>
    </div>
  </div>
</template>
<script>
import iconMap from '../iconMap/iconMap.vue'
import axios from 'axios'
import BScroll from "better-scroll";
import star from "@/components/star/star.vue";
export default {
  components: {
    star,
    iconMap
  },
  data() {
    return {
        seller:{}
    };
  },
  created() {
    this.initScroll();
  },
  computed: {
      calcWidth(){
         
        // debugger
        if(Object.keys(this.seller).length >0){
        console.log(this.seller)
        if(Object.keys(this.seller.pics).length >0){
            let len = this.seller.pics.length
            return len * 126
        }
        }else{
            // console.log(500)
            return 500
        }
        
    // return w
      }
  },
  methods: {
    initScroll() {
        axios.get('../../static/data.json').then((data) => {
              this.seller =  data.data.seller
            //   console.log(this.seller)
              this.$nextTick(() => {
         new BScroll(this.$refs.sellerWrapper);
        let bs = new BScroll(this.$refs.imgcontainer,{
            startX: 0,  // 配置的详细信息请参考better-scroll的官方文档，这里不再赘述
            click: true,
            scrollX: true,
            scrollY: false,
        })
        //  let w = document.getElementById('enterpriseinfo-container').clientWidth
        // console.log(bs);
      });
        })
    
    },
  },
};
</script>
<style lang="stylus" scoped>
.sellerWrapper {
  position: absolute;
  top: 177px;
  left: 0px;
  bottom: 0px;
  overflow: hidden;
  width: 100%;

  .content {
    .info {
      position: relative;
      margin: 0px 18px;
      padding: 18px 0px;

      .title {
        font-size: 14px;
        color: #07111b;
        line-height: 14px;
      }

      .star-container {
        display: inline-block;
        margin-bottom: 15px;
      }

      .favorite {
        position: absolute;
        top: 15px;
        right: 5px;
        text-align: center;

        & span:first-child {
          display: block;
          font-size: 22px;
        }

        & span:last-child {
          font-size: 12px;
        }
      }

      .remark {
        display: flex;

        & div {
          flex: 1;
          text-align: center;
          margin-top: 15px;

          h2 {
            font-size: 12px;
            color: #93999f;
            margin-bottom 5px
            line-height 12px
          }
           .num {
              font-size 22px
          } 
          .yuan{
              font-size 14px
          }
        }

        & div:nth-child(2) {
          border-left: 1px solid #ccc;
          border-right: 1px solid #ccc;
        }
        // .remark-item{
        //    & span:first-child {
        //         font-size 18px
        //     }
        // }
      }
    }
    .activities{
        margin 15px
        .bulletin{
            .content{
                margin 7px 10px
                font-size 13px 
                line-height 24px
                color #f01414
            }
        }
    }
    .supports{
        ul {
            margin 0px 15px
        }
        
        & li {
            // margin 15px
            border-top 1px solid #ccc
            font-size 14px
            padding 18px 12px
            
            & span:first-child{
                width 16px
                height 16px
                vertical-align top
            }
            & span:last-child{
                color #666
                font-size 13px
            }
        }
    }
    .seller-img{
        margin 15px
        & h2 {
            font-size 14px
            line-height 14px 
            margin-bottom 15px
        }
        .content{
            overflow hidden
            & img{
                margin-right 6px
                display line-block
                width 120px
                height 90px
            }
        }
    }
    .seller-info{
      h2 {
        margin 15px
      }
      ul {
        margin 15px
        li {
          padding  18px 0px
          font-size 12px
          line-height 16px
          border-bottom 1px solid rgba(7,17,27,0.1)
          color #07111b
          font-weight 200
        }
      }
    }
  }
}
</style>