<template>
  <div class="seller"  ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
          </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
          </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
            </div>
          </li>
        </ul>
        <div class="favorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <div class="title">公告与活动</div>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item" v-for="(item,index) in seller.supports">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picwrapper">
          <ul class="pic-list" ref="piclist">
            <li v-for="(pic,index) in seller.pics" class="pic-item">
              <img :src="pic" width="120px" height="90px">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <div class="title border-1px">商家信息</div>
        <ul>
          <li class="info-item" v-for="(info,index) in seller.infos">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import star from '../../components/star/star.vue'
  import split from '../../components/split/split.vue'
  export default {
      props:{
        seller:{
          type:Object
        }
      },
    data(){
      return{
        favorite:false
      }
    },
    computed:{
      favoriteText(){
        return this.favorite ? '已收藏':'未收藏'
      }
    },
    mounted: function () {
      this.$nextTick(function () {
        // 代码保证 this.$el 在 document 中
        this._initScroll()
        this._initPics()

      })
    },
    watch:{
      'seller'(){
        this._initScroll()
        this._initPics()
      }
    },
    components:{
        star,
        split
    },
    methods:{
      _initScroll(){
        if(!this.scroll){
          console.log('12213')
          this.scroll = new BScroll(this.$refs.seller,{
            click:true
          })
        }else{
          this.scroll.refresh()
        }
      },
      _initPics(){
        if(this.seller.pics){
          console.log('12213')
          let picWidth = 120
          let margin = 6
          let width = (picWidth+margin)*this.seller.pics.length - margin
          this.$refs.piclist.style.width = width + 'px'
            if(!this.picScroll){
              this.picScroll = new BScroll(this.$refs.picwrapper,{
                scrollX:true,
                eventPassthrough:'vertical'
              })
            }else {
              this.picScroll.refresh()
            }
        }
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      }
  }

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .seller
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
      position relative
      padding 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .desc
        padding-bottom 18px
        line-height 18px
        font-size 0px
        border-1px(rgba(7,17,27,0.1))
        .star
          display inline-block
          margin-right 8px
          vertical-align top
        .text
          margin-right 12px
          font-size 10px
          color rgb(77,85,93)
          display inline-block
          vertical-align top
      .remark
        display flex
        padding-top 18px
        .block
          flex 1
          text-align center
          border-right 1px solid rgba(7,17,27,0.1)
          &:last-child
            border none
          h2
            margin-bottom 4px
            line-height 10px
            font-size 10px
            color rgb(147,153,159)
          .content
            line-height 24px
            font-size 10px
            color rgb(7,17,27)
            .stress
              font-size 24px

      .favorite
        position absolute
        right 18px
        top 18px
        text-align center
        .icon-favorite
          display block
          margin-bottom 4px
          color #d4d6d9
          line-height 24px
          font-size 24px
          &.active
            color rgb(240,20,20)

    .bulletin
      padding 18px 18px 0 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .content-wrapper
        padding 0 12px 16px 12px
        border-1px(rgba(7,17,27,0.1))
        .content
          line-height 24px
          font-size 12px
          color rgb(240,20,20)
      .supports
        .support-item
          padding 16px 12px
          border-1px(rgba(7,17,27,0.1))
          font-size 0
          &:last-child
            border-none()
          .icon
            vertical-align top
            display inline-block
            width 16px
            height 16px
            margin-right 6px
            background-size 16px 16px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_4')
            &.discount
              bg-image('discount_4')
            &.guarantee
              bg-image('guarantee_4')
            &.invoice
              bg-image('invoice_4')
            &.special
              bg-image('special_4')
          .text
            line-height 16px
            font-size 12px
            color rgb(7,17,27)

    .pics
      padding 18px
      .title
        margin-bottom 12px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .pic-wrapper
        overflow hidden
        width 100%
        white-space nowrap
        .pic-list
          font-size 0
          .pic-item
            display inline-block
            margin-right 6px
            width 120px
            height 90px
            &:last-child
              margin 0
    .info
      padding 18px 18px 0 18px
      .title
        padding-bottom 12px
        line-height 14px
        border-1px(rgba(7,17,27,0.1))
        color rgb(7,17,27)
        font-size 14px
      .info-item
        padding 16px 12px
        line-height 16px
        border-1px(rgba(7,17,27,0.1))
        color rgb(7,17,27)
        font-size 12px
        &:last-child
          border-none()

</style>
