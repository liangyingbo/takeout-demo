<template>
    <div class="cartcontrol">
        <transition name='fadeRotate'>
            <div class="cart-decrease" v-show="food.count > 0" @click.stop.prevent="decreaseCart($event)">
                <span class="icon-remove_circle_outline inner"></span>
            </div>
        </transition>
        <div class="cart-count" v-show="food.count >0">{{food.count}}</div>
        <div class='cart-add' @click.stop.prevent="addCart($event)">
            <i class="icon-add_circle"></i>
        </div>
    </div>
</template>
<script>
import Vue from 'vue'
export default {
    props:{
        food:Object
    },
    data() {
     return{

     }   
    },
    created(){

    },
    methods:{
        addCart(e){
            //  debugger
            if(!e._constructed){
                return
            }
            if(!this.food.count){
                Vue.set(this.food,'count', 0)
            }
            this.food.count ++;
            this.$root.eventHub.$emit('cart.add', e.target)
        },
        decreaseCart(e){
            // debugger
            if(!e._constructed || !this.food.count){
                return
            }
            this.food.count --;
        }
    }

}
</script>
<style lang="stylus" scoped>
.cartcontrol  
    z-index 5
    .cart-decrease
        display inline-block
        color: #00a0dc;
        transition: all .6s linear
        .inner
            font-size 24px
            line-height 24px
            transition all 0.4s linear
        &.fadeRotate-enter-active, &.fadeRotate-leave-active
            transform translate3d(0,0,0)
            .inner
              display inline-block
              transform rotate(0)
        &.fadeRotate-enter, &.fadeRotate-leave-active
            opacity: 0
            transform translate3d(24px,0,0)
            .inner
              transform rotate(180deg)
    .cart-count
        display inline-block
        font-size 15px
        line-height 24px
        padding  0px 6px
        vertical-align top
        color: #93999f;
    .cart-add
        display inline-block
        font-size 24px
        line-height 24px
        color: #00a0dc;
</style>