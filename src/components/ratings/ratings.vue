<template>
  <div class="ratingsWrapper" ref="ratingsWrapper">
    <div>
      <div class="info" ref="content">
        <div class="info-left">
          <span>{{seller.score}}</span>
          <span>综合评分</span>
          <span>高于周边商家{{seller.rankRate}}%</span>
        </div>
        <div class="info-right">
          <div style="display: flex">
            <span
              style="
                font-size: 12px;
                line-height: 18px;
                margin: 0px 13px 6px 0px;
              "
              >服务态度</span
            >
            <div style=""><star :size="36" :score="seller.serviceScore"></star></div>
            <span
              style="
                font-size: 12px;
                color: #f90;
                line-height: 18px;
                margin-left: 5px;
              "
              >{{seller.serviceScore}}</span
            >
          </div>
          <div style="display: flex">
            <span
              style="
                font-size: 12px;
                line-height: 18px;
                margin: 0px 13px 6px 0px;
              "
              >服务态度</span
            >
            <div style=""><star :size="36" :score="seller.foodScore"></star></div>
            <span
              style="
                font-size: 12px;
                color: #f90;
                line-height: 18px;
                margin-left: 5px;
              "
              >{{seller.foodScore}}</span
            >
          </div>

          <div style="text-align: left; font-size: 12px; margin-top: 4px">
            <span>送达时间</span
            ><span style="color: gray; margin-left: 7px">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <div class="divider"></div>
      <div class="main">
        <div class="tag">
          <!-- <span class="all">全部</span>
          <span class="recommend">推荐</span>
          <span class="complaint">吐槽</span> -->
          <span v-for="(item,index) in classifyArr" class="item" :class="{'active':item.active,'bad':index ==2,'badactive':index ==2&&item.active}" @click="filterEnval(item)">
              {{item.name}}{{item.count}}
          </span>
        </div>
        <div class="classify">
          <span class="icon-check_circle on"></span>
          <span>只看有内容的评价</span>
        </div>
      </div>
      <div class="evel-list">
        <ul>
          <li v-for="item in this.ratings">
            <div class="content">
              <div class="left">
                <div class="img">
                  <img :src="item.avatar" width="28" alt="" />
                </div>
              </div>
              <div class="right">
                <div style="font-size: 12px">
                  <span> {{ item.username }}</span>
                  <span style="right: 10px; position: absolute; color: gray">
                    {{ item.rateTime | time }}</span
                  >
                </div>
                <div style="margin: 6px 0px">
                  <div style="display: inline-block">
                    <star :size="24" :score="item.score"></star>
                  </div>
                  <span style="font-size: 13px; color: gray"
                    >{{ item.deliveryTime }}分钟送达</span
                  >
                </div>
                <div style="font-size: 12px; color: #07111b; line-height: 19px;margin-bottom:8px">
                  {{ item.text }}
                </div>
                <div>
                  <span class="icon icon-thumb_up"></span>
                  <span
                    style="
                      border: 1px solid rgba(7, 17, 27, 0.1);
                      color: #93999f;
                      margin-right: 8px;
                      padding:2px 6px;
                      font-size: 13px;
                      display: inline-block;
                      line-height: 14px;
                      margin-top:4px
                    "
                    v-for="dish in item.recommend"
                    >{{ dish }}</span
                  >
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import star from "@/components/star/star";
import BScroll from "better-scroll";
// import '../../filter/time'
import "../../filter/time.js";
export default {
  components: {
    star,
  },
  data() {
    return {
      seller: [],
      ratings: [],
       classifyArr: [{
        name: '全部',
        count: 0,
        active: true
      }, {
        name: '推荐',
        count: 0,
        active: false
      }, {
        name: '吐槽',
        count: 0,
        active: false
      }],
    };
  },
  created() {
    this._initScroll();

  },
  computed:{

  },
  methods: {
    _initScroll() {
      axios.get("../../static/data.json").then((res) => {
        this.seller = res.data.seller;
        this.ratings = res.data.ratings;
        this.getRatingsCount()
        console.log(res.data);
        this.$nextTick(() => {
          let bs = new BScroll(this.$refs.ratingsWrapper, {click:true});
           bs.refresh()
          console.log(bs);
        });
      });
    },
    getRatingsCount(){
       this.classifyArr.forEach((data, index) => {
        if (index) {
          data.count = this.ratings.filter((temp) => temp.rateType === index - 1).length
        } else {
          data.count = this.ratings.length
        }
      })
    },
    filterEnval(item){
      // debugger
      // console.log(item)
      this.classifyArr.map(x => {
        return x.active = false;
      })
      item.active = true 
    }
  },
};
</script>
<style lang="stylus" scoped>
.ratingsWrapper {
  // overflow hidden
  position: absolute
  top: 174px
  bottom: 0
  left: 0
  width: 100%
  overflow: hidden
  .info {
    width: 100%;
    display: flex;
    text-align: center;

    .info-left {
      flex: 0 0 135px;
      margin: 18px 0px;
      border-right: 1px solid rgba(7, 17, 27, 0.1);

      & span:nth-child(1) {
        // background purple
        display: block;
        font-size: 24px;
        color: #f90;
        margin-bottom: 6px;
      }

      & span:nth-child(2) {
        font-size: 12px;
        color: #07111b;
        display: block;
        margin-bottom: 6px;
      }

      & span:nth-child(3) {
        font-size: 12px;
        color: #07111b;
        display: block;
        margin-bottom: 6px;
      }
    }

    .info-right {
      flex: 1;
      // background-color purple
      margin: 18px 24px;
    }
  }

  .main {
    .tag {
      margin: 0px 18px;
      padding: 22px 0px;
      font-size: 14px;
      border-bottom: 1px solid rgba(0, 160, 220, 0.2);
         .item{
              padding: 5px 13px;
              margin-right: 5px;
              background:rgba(0,160,220,0.2)

              &.bad{
                  background: rgba(77,85,93,0.2)
               
              }
              &.active{
                color: #fff;
                background: #00a9dc;
              }
              &.badactive{
                background #4d555d
              }
         }

    //   .all {
    //     padding: 5px 13px;
    //     background: #00a9dc;
    //     color: #fff;
    //     margin-right: 5px;
    //   }

    //   .recommend {
    //     padding: 5px 13px;
    //     background: rgba(0, 160, 220, 0.2);
    //     color: #4d555f;
    //     margin-right: 5px;
    //   }

    //   .complaint {
    //     background: rgba(77, 85, 93, 0.2);
    //     padding: 5px 13px;
    //     color: #4d555f;
    //   }
    }

    .classify {
      width: 100%;
      padding: 15px;
      border-bottom: 1px solid #666;
      font-size: 12px;

      & span:first-child {
        font-size: 22px;
        vertical-align: sub;
        color: green;
      }

      & span:last-child {
        color: #666;
      }
    }
  }

  .evel-list {
    ul {
      li {
        .content {
          display: flex;
          position: relative;
          border-bottom: 1px solid #e9dddd;
          padding 18px 0px
          margin 0 18px
          .left {
            flex: 0 0 28px;
            // background: pink;
            margin-right: 10px;

            img {
              width: 28px;
              height: 28px;
              border-radius: 50%;
            }
          }

          .right {
            flex: 1;
            // background-color: purple;
          }
        }
      }
    }
  }
}
</style>