<template>
    <div class="goods">
       
        <div class="menu-wrapper" ref="menuwrapper">
            <ul>
                <li v-for="(item ,index) in goods" :class="{'menu-item-active': index===calcI}" class="menu-item" @click="menuClick(index)">
                    <span class="text">
                        <icon-map v-if='item.type >0' :iconType="item.type"></icon-map>
                        {{item.name}}
                        </span>
                </li>
            </ul>
        </div>
    <div class="foods-wrapper" id="wrapper" ref="foodswrapper">
        <ul>
             <!-- <div>{{selectedfood}}</div> -->
            <li v-for="item in goods" class="food-list-hook">
                <h1>{{item.name}}</h1>
                <ul>
                    <li v-for="food in item.foods" class="food-item" @click="goDetail(food)">
                        <div class="icon">
                            <img :src="food.icon" alt="" width="50" height="50">
                        </div>
                        <div class="content">
                            <h2>{{food.name}}</h2>
                            <p class="description">{{food.description}}</p>
                            <div class="sell-info">
                                <span>月售{{food.sellCount}}份</span>
                                <span> 好评率{{food.rating}}%</span>
                            </div>
                            <div class="price">
                                <span>￥{{food.price}}</span>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <cartcontrol :food='food'></cartcontrol>
                            </div>
                        </div>
                    </li>
                </ul>
            </li>
        </ul>
    </div>
    <shopCart :deliveryPrice='seller.deliveryPrice' :minPrice='seller.minPrice' :selectFoods='selectFoods' @empty='empty'></shopCart>
    <food-detail :food='selectedFood' v-if="selectedFood" ref="myfood"></food-detail>
    </div>
</template>
<script>
import foodDetail from '@/components/foodDetail/foodDetail'
import iconMap from '@/components/iconMap/iconMap'
import axios from "axios"
import BScroll from 'better-scroll'
import FoodDetail from '../foodDetail/foodDetail'
import cartcontrol from '@/components/cartcontrol/cartcontrol'
import shopCart from '@/components/shopCart/shopCart'
import Vue from 'vue'

export default {
    props:{
        seller:Object
    },
    data(){
        return {
            goods:[],
            selectedFood: '',
            foodwrapper:null,
            foodheight:[],
            selectIndex:0,
            scrollY:0
        }
    },
    created(){
        axios.get('../../../static/data.json').then(res => {
            this.goods = res.data.goods
            // console.log(JSON.stringify(this.goods))
            this.$nextTick( () => {
                this._initScroll()
                this.calcualt() 
            })
        })
    },
    mounted(){

    },
    computed:{  
        calcI(){
               for(let i =0; i < this.foodheight.length;i++ ){
                   let top = this.foodheight[i]
                   let bottom = this.foodheight[i+1]
                   if(top <= this.scrollY  && this.scrollY < bottom){
                       return i
                   } 
               }
        },
        selectFoods(){
            // debugger
            let f = []
            this.goods.forEach(x => {
                if(x.foods){
                     x.foods.forEach( y => {
                    if(y.count && y.count > 0){
                        f.push(y)
                    }
                })
                }
               
            })

            return f
        }
    },
    methods:{
        _initScroll(){
             new BScroll(this.$refs.menuwrapper,{
              click:true
            })
         this.foodwrapper = new BScroll(this.$refs.foodswrapper,{
                click:true,
                probeType:3
            })

         this.foodwrapper.on('scroll', (position) => {
            //  this.scrollY = position.y
            this.scrollY = Math.abs(position.y)
            //  console.log(this.scrollY)
         })
        
        },
        calcualt() {
           let list = this.$refs.foodswrapper.querySelectorAll('.food-list-hook')
           let height = 0
           this.foodheight.push(height)
           list.forEach(e => {
               height += e.offsetHeight
               this.foodheight.push(height)
           });
           console.log(this.foodheight)
         },
        goDetail(food){
            this.selectedFood = food;
             this.$nextTick(() =>{
                this.$refs.myfood.showToggle()
                // console.log(this.$refs.myfood)
             })
          
        },
        menuClick(index){
            // this.selectIndex = index
              this.foodwrapper.scrollTo(0,-this.foodheight[index])
            // this.foodwrapper.on('scroll',(pos) =>{
            //    this.foodwrapper.scrollTo(0,-800)
            // })

        },
        empty(){
            let list  = this.selectFoods
            list.map((x) =>{
                x.count = 0
            })
             
        }
    },
    components:{
        foodDetail,
        iconMap,
        FoodDetail,
        cartcontrol,
        shopCart
    }
}
</script>
<style lang="stylus">
@import '../../common/stylus/mixin'
.goods
    display flex
    position absolute
    overflow hidden
    width 100%
    bottom 46px
    top 175px
    .menu-wrapper
        flex 0 0 80px
        width 80px
        background: #f3f5f7;
        .menu-item
            width 56px
            height 54px
            display table         
            padding 0 12px
            position relative
            .text
                display table-cell
                font-size 12px
                vertical-align middle
                line-height 16px
        .menu-item:after
            position absolute
            width 56px
            content ''
            border-bottom 1px solid rgba(7,17,27,0.1)
            bottom 0
            left 12px
        .menu-item-active
            background-color #fff
    .foods-wrapper
        flex 1
        .food-list-hook
            h1
                height: 26px;
                line-height: 26px;
                padding-left: 12px;
                font-size: 12px;
                color: #93999f;
                background: #f3f5f7;
                border-left: 2px solid #d9dde1;
                
            .food-item
                display flex
                position relative
                padding 18px 0px
                margin 0px 18px
                border-bottom 1px solid rgba(7,17,27,0.1)
                .icon
                    flex 0 0 57px
                .content
                    margin-left 6px
                    flex 1
                    h2
                        font-size 14px
                        font-weight 700
                    p
                        margin 7px 0px
                        font-size 12px
                        color #93999f
                    .sell-info
                        font-size 12px
                        color #93999f
                        margin 6px 0px 2px 0px 
                    .price
                        font-size: 10px;
                        font-weight: 700;
                        line-height: 24px;
                        span
                            color red
                    .cartcontrol-wrapper
                        position absolute
                        bottom 12px
                        right 0px  
                        z-index 20           
               
                    
</style>