<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <ul class='tab'>
        <li><router-link to='/goods'>商品</router-link></li>
        <li><router-link to='/ratings'>评价</router-link></li>
        <li><router-link to='/seller'>商家</router-link></li>
    </ul>
    <router-view :goods="goods" :seller="seller" :ratings="ratings">
    </router-view>
  </div>
</template>

<script type="text/ecmascript6">
    import header from './components/header/header'
    import axios from 'axios'

  
    export default{
      data(){
        return {
          seller:{},
          goods:{},
          ratings:{}
        }
      },
      created() {
        // this指向vue实例
        var that = this
        axios.get('../static/data.json').then(function (response){
          that.seller = response.data.seller
          that.goods = response.data.goods
          that.ratings = response.data.ratings
        }).catch(function (error){
        })
      },
      components: {
        'v-header':header,
      }
    }
</script>

<style lang="scss" scope>
  @import url('./common/css/index.scss');
  #app{
    display: flex;
    flex-direction: column;
    height: 100vh;
  }
  .tab {
        list-style: none;
        display: flex;
        line-height: (80rem/75);
        flex:0 0 (80rem/75);
        font-size: (28rem/75);
        border:1px solid rgba(7,17,27,0.2);
        border-left: none;
        border-right: none;
        li {
            flex-grow: 1;
            text-align: center;
            a{
              text-decoration: none;
              display:inline-block;
              width: 100%;
            }
        }
    }
  .router-link-exact-active{
    color: rgb(240, 20, 20)
  }
</style>