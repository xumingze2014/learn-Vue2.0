<template>
  <div class="cartcontrol">
    <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="decreaseCart">

    </div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default{

    props :{
      food:{
        type:Object
      }
    },
    methods :{
      addCart(event){
        if (!event._constructed){
          return
        }
        console.log('click')
        if (!this.food.count){
          Vue.set(this.food,'count',1)
        }else{
          this.food.count++
        }
      },
      decreaseCart(event){
        if (!event._constructed){
          return
        }
        console.log('click')
        if (this.food.count){
          this.food.count--
        }
      }
    }
  }
</script >


<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .cart-decrease
      color rgb(0,160,220)
      line-height 24px
      font-size 24px
      display inline-block
      padding 6px
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px
      line-height 24px
      text-align center
      font-size 10px
      color rgb(147,153,159)
    .cart-add
      padding 6px
      color rgb(0,160,220)
      font-size 24px
      line-height 24px
      display inline-block
</style>
