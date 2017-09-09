<template>
  <div class="header">
    <div class='logo'><img :src="seller.avatar" alt=""></div>
    <div class='information'>
      <p class='name'><span class='brand'></span>{{seller.name}}</p>
      <p class='description'>{{seller.description}}/{{seller.deliveryTime}}分钟送达</p>
      <p v-if='seller.supports'  @click='showDetail' class='support'>{{seller.supports[0].description}}<span>{{seller.supports.length}}个</span></p>
    </div>
    <div class='notice'></div>
    <div v-show='detailShow' class='detail'>
        <section></section>
        <footer><i @click='hideDetail' class="icon-close"></i></footer>
    </div>
  </div>
</template>

<script  type="text/ecmascript6">
    export default{
      data(){
        return{
          detailShow:false
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
      }
    }
</script>

<style lang="scss" scope>
  .header{
    font-size: 0;//消除inline-block空白
  }
  .logo,.information{
    display: inline-block;
  }
  .logo{
    width: (128rem/75);
    height: (128rem/75);
    img{
      width: 100%;
      height: 100%;
    }
  }
  .information{
    color:rgb(255,255,255);
    background-color: #bbb;
    line-height: 24px;
    font-weight: 200;
    .name{
      font-size: (32rem/75);
      font-weight: bold;
      line-height: 36px;
    }
    .description{
      font-size: 24px;
    }
    .support{
      font-size: 20px;
    }
  }
  .detail{
    display: flex;
    flex-flow: column;
    background:#000;
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
    }
    footer{
      width: 100vw;
      height: (128rem/75);
      text-align: center;
      font-size: (65rem/75);
      color: rgba(255,255,255,0.5)
    }
  }
    

  
</style>