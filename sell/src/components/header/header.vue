<template>
  <div class="header">
    <div class='logo'><img :src="seller.avatar" alt=""></div>
    <div class='information'>
      <p class='name'><span class='brand icon'></span>{{seller.name}}</p>
      <p class='description'>{{seller.description}}/{{seller.deliveryTime}}分钟送达</p>
      <p v-if='seller.supports'  @click='showDetail' class='support'>
        <span :class="[supportIcon[seller.supports[0]['type']]]" class='icon'></span>
        {{seller.supports[0].description}}
      </p>
      <span v-if='seller.supports'  @click='showDetail' class="support-num" ><p>{{seller.supports.length}}个<i class="icon-keyboard_arrow_right"></i></p>
      </span>
    </div>
    <div class='notice'></div>
    <div v-show='detailShow' class='detail'>
        <section>
          <h3 class='name'>{{seller.name}}</h3>
          <v-star :score='seller.score' :size='48'></v-star>
          <div class="partition" v-if='seller.supports'>
            <hr></hr>
            <h4>优惠信息</h4>
            <hr></hr>
          </div>
          <ul class="supportDetail" v-if='seller.supports'>
            <li v-for='(supportItem,index) in seller.supports' :key='index'>
              <span :class="supportIcon[supportItem['type']]" class="icon"></span>
              {{supportItem.description}}</li>
          </ul>
          <div class="partition" v-if='seller.bulletin'>
            <hr></hr>
            <h4>商家公告</h4>
            <hr></hr>
          </div>
          <p v-if='seller.bulletin' class='bulletin'>{{seller.bulletin}}</p>
        </section>
        <footer><i @click='hideDetail' class="icon-close"></i></footer>
    </div>
  </div>
</template>

<script  type="text/ecmascript6">
    import star from '../star/star'
    export default{
      data(){
        return{
          detailShow:false,
          supportIcon:['decrease','discount','special','invoice','guarantee']
        }
      },
      methods: {
        showDetail(){
          this.detailShow = true
        },
        hideDetail(){
          this.detailShow = false
        }
      },
      props: {
        seller:{
          type:Object
        }
      },
      components: {
        'v-star':star
      }
    }
</script>

<style lang="scss" scope>
  @import '../../common/css/custom';
  @import '../../common/css/mixin';
  .header{
    font-size: 0;//消除inline-block空白
    position: relative;
    background: rgba(7, 17, 27, 0.5);
    .logo,.information{
      display: inline-block;
      vertical-align: top;
    }
    .logo{
      margin: (48rem/75)(32rem/75)(36rem/75)(48rem/75);
      width: (128rem/75);
      height: (128rem/75);
      img{
        width: 100%;
        height: 100%;
      }
    }
    .information{
      margin-top: (48rem/75);
      color:rgb(255,255,255);
      line-height: 24px;
      font-weight: 200;
      .name{
        margin-top: (4rem/75);
        margin-bottom: (16rem/75);
        font-size: 32px;
        font-weight: bold;
        line-height: 36px;
        .brand{
          margin-right: (12rem/75);
          vertical-align: bottom;
          width: 60px;
          height: 36px;
        }
      }
      .description{
        font-size: 24px;
        margin-bottom: (20rem/75);
      }
      .support{
        font-size: 20px;
        .icon{
          display: inline-block;
          width: 24px;
          height: 24px;
          vertical-align: bottom;
          letter-spacing: 0;
        }
      }
      .support-num{
          // display: inline-block;
          position: absolute;
          right: (24rem/75);
          bottom:(36rem/75);
          height: (48rem/75);
          margin-bottom: (-10rem/75);
          background: rgba(0,0,0,0.2);
          font-size: 20px;
          padding-left: (16rem/75);
          padding-right: (16rem/75);
          border-radius: (24rem/75);
          p{
            display: inline;
            line-height: 48px;
            font-weight: 200;
            color: rgb(255,255,255);
            i{
              vertical-align: middle;
            }
          }
        }
    }
    .detail{
      display: flex;
      flex-flow: column;
      background: rgba(7,17,27,0.8);
      -webkit-backdrop-filter:blur(10px);
      backdrop-filter:blur(10px);
      position: fixed;
      top: 0;
      left:0;
      z-index: 100;
      width:100vw;
      min-height: 100vh;
      overflow: auto;
      // -webkit-scrollbar {
      //   display:none;
      // }
      section{
        flex:1;
        text-align: center;
        div{
          margin: 0 auto;
        }
        .name{
          margin-top: (128rem/75);
        }
        .star{
          width: (374rem/75);
          margin-top: (32rem/75);
        }
        .partition{
          display: flex;
          margin-top: (56rem/75);
          margin-bottom: (48rem/75);
          width: (606rem/75);
          h4{
            flex: 1;
          }
          hr{
            margin: 0;
            width: (224rem/75);
            height: 3px;
            // line-height: (28rem/75);
            border: none;
            background-color: rgba(255,255,255,0.2);
            // vertical-align: middle;
            align-self:center;
          }
          h4{
            padding: 0 -(24rem/75) 0;
            width: (110rem/75);
          }
        }
        .supportDetail{
          width: (558rem/75);
          margin: 0 auto;
          text-align: left;
          font-size: 24px;
          font-weight: 200;
          color: rgb(255,255,255);
          li{
            list-style: none;
            line-height: 24px;
            margin-bottom: 24px;
            span{
              vertical-align: text-bottom;
              width: 32px;
              height: 32px;
            }
          }
        }
        .bulletin{
          @extend .supportDetail;
          line-height: 48px;
        }
      }
      footer{
        width: 100vw;
        height: (128rem/75);
        text-align: center;
        font-size: (65rem/75);
        color: rgba(255,255,255,0.5)
      }
      h3,h4{
        color: rgb(255,255,255);
        font-weight: 700;
      }
    }
    .icon{
      display: inline-block;
      background-size: cover;
      &.decrease{
        @include bg-img('decrease_1');
      }
      &.guarantee{
        @include bg-img('guarantee_1');
      }
      &.discount{
        @include bg-img('discount_1');
      }
      &.invoice{
        @include bg-img('invoice_1');
      }
      &.special{
        @include bg-img('special_1');
      }
      &.brand{
        @include bg-img('brand');
      }
      &.bulletin{
        @include bg-img('bulletin');
      }
    }
  }
// .blur{
//   filter:blur(10px);
// }
</style>