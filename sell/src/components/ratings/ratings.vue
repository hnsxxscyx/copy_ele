<template>
  <div class="ratings">
    <div class="ratings-wrapper" ref="ratingsWrapper">
      <div>
        <div class="info">
          <div class="score">
            <h1>{{seller.score}}</h1>
            <p class="score-info">综合评分</p>
            <p class="score-compare">高于周边商家{{seller.rankRate}}%</p>
          </div>
          <div class="star-info">
            <div><p>服务态度</p>
              <span class="star-wrapper">
                <star :score="seller.serviceScore" :size="36"></star>
              </span>
              <p class="score-detail">{{seller.serviceScore}}</p>
            </div>
            <div><p>商品评分</p>
              <span class="star-wrapper">
                <star :score="seller.foodScore" :size="36"></star>
              </span>
              <p class="score-detail">{{seller.foodScore}}</p>
            </div>
            <div><p>送达时间</p>
              <p class="delivery-time">
                {{seller.deliveryTime}}分钟
              </p>
            </div>
          </div>
        </div>
        <div class="ratings-detail">
          <ratingsDetails v-on:contentChange="_initScroll()" :ratings="ratings" :selectDesc="selectDesc">
          </ratingsDetails>
        </div>
      </div>
    </div>
    <shopcart :goods="goods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
  </div>
</template>

<script  type="text/ecmascript6">
    import star from "../star/star"
    import ratingsDetails from "../ratingsDetails/ratingsDetails"
    import bus from '../eventBus'
    import shopcart from '../shopcart/shopcart'
    import BScroll from 'better-scroll'

    export default{
      components: {
        star,
        ratingsDetails,
        shopcart
      },
      data () {
        return {
          selectDesc:{
                      all:'全部',
                      pleased:'满意',
                      unpleased:'不满意'
                    }
        }
      },
      props:{
        seller:{
          type:Object
        },
        ratings:{
          type:Array
        },
        goods:{

        }
      },
      mounted () {
        bus.$emit("countChange")
        this._initScroll()
      },
      methods: {
        _initScroll(){
          this.scroll = new BScroll(this.$refs.ratingsWrapper,{
            click:true
          })
        }
      }
    }
</script>

<style lang="scss" scope>
  .ratings{
    background-color: #f3f5f7;
    display: flex;
    flex: 1;
    flex-direction: column;
    .ratings-wrapper{
      overflow: hidden;
      flex-grow: 1;
    }
    .info{
      background-color: #fff;
      padding: (36rem/75) (48rem/75);
      margin-bottom: (36rem/75);
      border-bottom: 2px solid rgba(7,17,27,0.1);
      display: flex;
      .score{
        border-right: 2px solid rgba(7,17,27,0.1);
        text-align: center;
        flex-basis:(275rem/75);
        h1{
          font-size: (48rem/75);
          color: rgb(255,153,0);
          line-height: (56rem/75);
        }
        .score-info{
          font-size: (24rem/75);
          color: rgb(7,17,27);
          line-height: (24rem/75);
          margin:(12rem/75) 0 (16rem/75) 0;
          font-weight: 700;
        }
        .score-compare{
          font-size: (20rem/75);
          font-weight: 600;
          color:rgba(7,17,27,0.5);
          line-height: (20rem/75);
          margin-bottom: (12rem/75);
        }
      }
      .star-info{
        flex-basis:(474rem/75);
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        >div{
          display: flex;
          padding-left:(48rem/75);
        }
        p{
          font-size: (24rem/75);
          font-weight: 700;
          line-height: (36rem/75);
          white-space:nowrap;
        }
        .star-wrapper{
          flex-basis: (197rem/75);
          flex-grow: 0;
          margin: 0 (24rem/75);
          // 这个padding用来平衡图片的不居中问题
          padding-top: (2rem/75);
        }
        .score-detail{
          color: rgb(255,153,0);
        }
        .delivery-time{
          font-size: (24rem/75);
          color: rgb(147,153,159);
          margin-left: (24rem/75);
        }
      }
    }
  }
</style>