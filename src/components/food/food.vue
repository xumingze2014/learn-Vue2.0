<template>
  <transition name="fade">
      <div v-show="showFlag" class="food" ref="food" >
        <div class="food-content">
          <div class="image-header">
            <img :src="food.image">
            <div class="back" @click="hide">
              <i class="icon-arrow_lift"></i>
            </div>
          </div>
          <div class="content">
            <h1 class="title">{{food.name}}</h1>
            <div class="deatil">
              <span class="sell-count">月售{{food.sellCount}}份</span>
              <span class="rating">好评率{{food.rating}}%</span>
            </div>
            <div class="price">
              <span class="now">￥{{food.price}}</span>
              <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
            </div>
            <div class="cartcontrol-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
            <div @click="addFirst($event)" class="buy" v-show="!food.count || food.count===0">加入购物车</div>
          </div>
          <split v-show="food.info"></split>
          <div class="info" v-show="food.info">
            <h1 class="title"  v-show="food.info">商品信息</h1>
            <p class="text" v-show="food.info">{{food.info}}</p>
          </div>
          <split></split>
          <div class="rating">
            <h1 class="title">商品评价</h1>
            <ratingselect @select="selectRating" @toggle="toggleContent" :select-type="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
            <div class="rating-warpper">
              <ul v-show="food.ratings && food.ratings.length">
                <li v-show="needShow(rating.rateType,rating.text)" v-for="(rating,index) in food.ratings" class="rating-item">
                  <div class="user">
                    <span class="username">{{rating.username}}</span>
                    <img class="avatar" width="12" height="12" :src="rating.avatar">
                  </div>
                  <div class="time">{{rating.rateTime}}</div>
                  <p class="text">
                    <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                    {{rating.text}}
              </p>
                </li>
              </ul>
              <div class="no-rating" v-show="!food.ratings || !food.ratings.length"></div>
            </div>
          </div>
        </div>
      </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  const POSITIVE = 0
  const NEGATIVE = 1
  const ALL = 2
  import BScroll from 'better-scroll'
  import cartcontrol from '../../components/cartcontrol/cartcontrol.vue'
  import vue from 'vue'
  import split from '../../components/split/split.vue'
  import ratingselect from '../../components/ratingselect/ratingselect.vue'
  export default{
    props:{
      food:{
        type:Object
      }
    },
    data(){
      return {
        showFlag : false,
        selectType :ALL,
        onlyContent:true,
        desc:{
          all:'全部',
          positive:'推荐',
          nagative:'吐槽'
      }
    }
    },
    methods:{
      show(){
        this.showFlag = true
        this.selectType =ALL
        this.onlyContent=true
        this.$nextTick(() => {
          console.log('this.scroll')
          if(!this.scroll){
            this.scroll = new BScroll(this.$refs.food,{
              click: true
            })
            console.log('this.scroll true')
          }else{
            console.log('this.scroll false')
            this.scroll.refresh()
          }
        })
      },
      hide(){
        this.showFlag = false
      },
      addFirst(event){
        if(!event._constructed){
          return
        }
        this.$emit('add',event.target)
        vue.set(this.food,'count',1)
      },
      needShow(type,text){
        if (this.onlyContent && !text) {
          return false
        }
        if (this.selectType === ALL) {
          return true
        }else{
          return type === this.selectType
        }
      },
      selectRating(type){
        this.selectType = type
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      toggleContent(){
        this.onlyContent = !this.onlyContent
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      }
    },
    components:{
      cartcontrol,
      split,
      ratingselect
    }
  }

</script >


<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .food
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background white
    transform translate3d(0,0,0)
    &.fade-enter-active
      transition:  all 0.2s linear
      transform translate3d(0,0,0)
    &.fade-enter
      transition:  all 0.2s linear
      transform translate3d(100%,0,0)
    &.fade-leave-active
      transition:  all 0.2s linear
      transform translate3d(100%,0,0)
    &.fade-leave
      transition:  all 0.2s linear
      transform translate3d(0,0,0)
    .food-content
      .image-header
        position relative
        width 100%
        height 0
        padding-top 100%
        img
          position absolute
          top 0
          left 0
          width 100%
          height 100%
        .back
          position absolute
          left 0px
          top 0px
          .icon-arrow_lift
            display block
            padding 10px
            font-size 20px
            color white
      .content
        position relative
        padding 18px
        .title
          line-height 14px
          margin-bottom 8px
          font-size 14px
          font-weight 700
          color rgb(7,17,27)
        .detail
          margin-bottom 18px
          line-height 10px
          height 10px
          font-size 0
          .sell-count,.rating
            font-size 10px
            color rgb(147,153,159)
          .sell-count
            margin-right  12px
        .price
          font-weight 700
          line-height 24px
          .now
            margin-right 8px
            font-size 14px
            color : rgb(240,20,20)
          .old
            text-decoration line-through
            font-size 10px
            color rgb(147,153,159)
        .cartcontrol-wrapper
          position absolute
          bottom 12px
          right 12px
        .buy
          position absolute
          bottom 18px
          right 18px
          z-index 10
          height 24px
          line-height 24px
          padding 0 12px
          box-sizing border-box
          font-size 10px
          border-radius 12px
          background rgb(0,160,220)
          color white
    .info
      padding 18px
      .title
        line-height 14px
        margin-bottom 6px
        font-size 14px
        color: rgb(7,17,27)
      .text
        color: rgb(77,85,93)
        padding 0 8px
        font-size 12px
        color rgb(77,85,93)
    .rating
      padding-top 18px
      .title
        line-height 14px
        margin-bottom 6px
        font-size 14px
        color rgb(7,17,27)
      .rating-warpper
        padding 0 18px
        .rating-item
          position relative
          padding 16px 0
          border-1px(rgba(7,17,27,0.1))
          .user
            position absolute
            right 0px
            top 16px
            line-height 12px
            font-size 0px
            .username
              display inline-block
              margin-right 6px
              vertical-align top
              font-size 10px
              color rgb(147,153,159)
            .avatar
              border-radius 50%

          .time
            margin-bottom 6px
            line-height 12px
            font-size 10px
            color rgb(147,153,159)
          .text
            line-height 16px
            font-size 12px
            color rgb(7,17,27)
            .icon-thumb_up,.icon-thumb_down
              line-height 24px
              margin-right 4px
              font-size 12px
            .icon-thumb_up
              color rgb(0,160,220)
            .icon-thumb_down
              color rgb(147,153,159)
</style>
