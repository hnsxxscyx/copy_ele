<template>
  <div class="seller">
    <div class="seller-wrapper" ref="sellerWrapper">
      <div>
        <div class="info-basic">
          <div>
            <div class="info">
              <h4>{{seller.name}}</h4>
              <p class="info-data">
                <span class="star-wrapper">
                  <star :score="seller.score" :size="36"></star>
                </span>
                <span class="rating-count">({{seller.ratingCount}})</span>
                <span class="sell-count">月售{{seller.sellCount}}单</span>
              </p>
            </div>
            <div class="favorite" @click="isFavorite=!isFavorite">
              <i :class="[isFavorite?'favorite-active':'','icon-favorite']"></i>
              <p v-if="isFavorite">已收藏</p>
              <p v-if="!isFavorite">未收藏</p>
            </div>
          </div>
          <div>
            <div class="minPrice">
              <p class="info">起送价</p>
              <p>{{seller.minPrice}}<span class="string">元</span></p>
            </div>
            <div class="deliveryPrice">
              <p class="info">{{seller.description}}</p>
              <p>{{seller.deliveryPrice}}<span class="string">元</span></p>
            </div>
            <div class="deliveryTime">
              <p class="info">平均配送时间</p>
              <p>{{seller.deliveryTime}}<span class="string">分钟</span></p>
            </div>
          </div>
        </div>
        <div class="bulletin" v-if="seller.bulletin">
          <h4>公告与活动</h4>
          <p>{{seller.bulletin}}</p>
          <ul>
            <li v-for="(item,index) in seller.supports" :key="index" class="support-list">
              <icon :form="item['type']" class="icon"></icon>
              {{item.description}}
            </li>
          </ul>
        </div>
        <div class="seller-photo">
          <h4>商家实景</h4>
          <div class="photo" ref="photo">
            <div class="photo-wrapper">
              <img v-for="(src,index) in seller.pics" :key="index" :src="src" alt="实景图">
            </div>
          </div>
        </div>
        <div class="seller-info">
          <h4>商家信息</h4>
          <ul>
            <li v-for="(item,index) in seller.infos" :key="index" class="info-detail">
              <p>{{item}}</p>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <shopcart :goods="goods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
  </div>
</template>

<script type="text/ecmascript6">
import star from '../star/star'
import icon from '../icon/icon'
import shopcart from '../shopcart/shopcart'
import bus from '../eventBus'
import BScroll from 'better-scroll'

export default {
  components: {
    star,
    icon,
    shopcart
  },
  data () {
    return {
      isFavorite:false,
    };
  },
  props: {
    seller:{
          type:Object
        },
    ratings:{
    },
    goods:{
      
    }
  },
  methods: {
    _initScroll(){
      this.scroll = new BScroll(this.$refs.sellerWrapper,{
        click:true
      })
      this.photoScroll = new BScroll(this.$refs.photo,{
        scrollX:true
      })
    }
  },
  mounted() {
    bus.$emit("countChange")
    this._initScroll()
  }
}
</script>

<style lang="scss" scoped>
  .seller{
    background-color: #f3f5f7;
    display: flex;
    flex:1;
    flex-direction: column;
    .seller-wrapper{
      overflow: hidden;
      >div{
      >div{
        background: #fff;
        padding:(36rem/75);
        margin-bottom: (36rem/75);
        border: 2px solid rgba(7,17,27,0.1);
        border-left:none;
        border-right:none;
      }
      .info-basic{
        display: flex;
        flex-direction: column;
        border-top: none;
        >div{
          padding: (36rem/75);
          padding-bottom: 0;
          background: #fff;
        }
        >div:nth-of-type(1){
          display: flex;
          justify-content:space-between;
          .info,.favorite{
            padding-bottom: (36rem/75);
            border-bottom: 1px solid rgba(7,17,27,0.1);
          }
          .info{
            display: flex;
            flex-direction: column;
            flex-grow: 1;
            >h4{
              font-weight: 700;
              margin-bottom: (16rem/75);
            }
            .info-data{
              font-size: (20rem/75);
              display: flex;
              flex-direction: row;
              align-items: center;
              span{
                flex:none;
                color: rgb(77,85,93);
                line-height: (36rem/75);
                font-weight: 600;
              }
              .star-wrapper{
                padding-top: (3rem/75);
              }
              .rating-count{
                margin-left: (16rem/75);
              }
              .sell-count{
                margin-left: (24rem/75);
              }
            }
          }
          .favorite{
            .icon-favorite{
              color:#a3a5a7;
            }
            .favorite-active{
              color:#f01414;
            }
            p{
              font-size: (20rem/75);
              color: rgb(77,85,93);
              line-height: (20rem/75);
              font-weight: 600;
              text-align: center;
            }
          }
        }
        >div:nth-of-type(2){
          display: flex;
          text-align: center;
          padding-bottom:(36rem/75);
          >div{
            flex:1;
          }
          .deliveryPrice{
            border-left: 1px solid rgba(7,17,27,0.1);
            border-right: 1px solid rgba(7,17,27,0.1);
          }
          p{
            font-size: (48rem/75);
          }
          .string{
            font-size: (20rem/75);
          }
          .info{
            font-size: (20rem/75);
            color: rgb(147,153,159);
            line-height: (20rem/75);
          }
        }
      }
      .bulletin{
        padding-bottom: (32rem/75);
        >h4{
          font-weight: 700;
          margin-bottom: (16rem/75);
        }
        >p{
          padding: 0 (24rem/75);
          padding-bottom: (32rem/75);
          font-size: (24rem/75);
          font-weight: 200;
          color: rgb(240,20,20);
          line-height: (48rem/75);
        }
        ul{
          .support-list{
            padding: (32rem/75) (24rem/75);
            border-top: 1px solid rgba(7,17,27,0.1);
            .icon{
              width: (32rem/75);
              height: (32rem/75);
              margin-right: (12rem/75);
            }
            font-size: (24rem/75);
            font-weight: 200;
            color: rgba(7,17,27,1);
            line-height: (32rem/75);
          }
        }
      }
      .seller-photo{
        >h4{
          font-weight: 700;
          margin-bottom: (16rem/75);
        }
        .photo{
          height: 100%;
          white-space:nowrap;
          >.photo-wrapper{
            width: fit-content;
          }
          img{
            margin-right: (12rem/75);
            width: (240rem/75);
            height: (180rem/75);
          }
          &::-webkit-scrollbar{
            display:none;
          }
        }
      }
      .seller-info{
        >h4{
          font-weight: 700;
          margin-bottom: (24rem/75);
        }
        .info-detail{
          padding: (32rem/75) (24rem/75);
          border-top: 1px solid rgba(7,17,27,0.1);
          p{
            font-size: (24rem/75);
            font-weight: 200;
            color: rgba(7,17,27,1);
            line-height: (32rem/75);
          }
        }
        }
      }
    }
  }
</style>