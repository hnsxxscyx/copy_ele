<template>
  <div class="goods">
    <div class="goods-show">
      <div class="menu" ref='menu'>
        <ul class='typeList'>
          <li v-for='(goodsItem,index) in goods' :key='index' :class='{"active":currentIndex===index}'>
            <hr v-if='index!==0' class="cutline">
            <span class="text"><v-icon :form="goods[index]['type']" class='icon' v-if='goods[index].type>=0'></v-icon>{{goodsItem.name}}</span>
          </li>
        </ul>
      </div>
      <div class="foodsList" ref='foodsList'>
        <div class="wrapper">
          <ul v-for='(goodsItems,index) in goods' :key='index' class='foodsDetail'>
            <h5><hr>{{goodsItems.name}}</h5>
            <li v-for='(goodsItem,index) in goodsItems.foods' :key='index' @click='clickGoodsItems(goodsItem,$event)' class='goodsItem'>
              <hr v-if='index!==0' class="cutline">
              <div class="logo">
                <img :src='goodsItem.icon' alt="商品图">
              </div>
              <div class="info">
                <h4>{{goodsItem.name}}</h4>
                <p>{{goodsItem.description}}</p>
                <p>月售{{goodsItem.sellCount}}<span class="rating">好评率{{goodsItem.rating}}%</span></p>
                <p class="price"><span class="newPrice"><span class="symbol">￥</span>{{goodsItem.price}}</span><span v-if='goodsItem.oldPrice' class="oldPrice">￥{{goodsItem.oldPrice}}</span></p>
              </div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="goodsItem"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <shopcart :goods="goods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    <food ref='foodEle' :detail="foodDetail"></food>
  </div>
</template>

<script  type="text/ecmascript6">
  import BScroll from 'better-scroll'
  import icon from '../icon/icon'
  import shopcart from '../shopcart/shopcart'
  import cartcontrol from '../cartcontrol/cartcontrol'
  import food from '../food/food'
  import bus from '../eventBus'

  export default{
    data () {
      return {
        listHeight:[],
        position:{y:0},
        clickEle:null,
        foodDetail:{}
      }
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
    mounted() {
      // this指向vue实例
      // var that = this
      // axios.get('../static/data.json').then(function (response){
      //   that.goods = response.data.goods
      //   that.seller = response.data.seller
        
      // }).catch(function (error){
      // })
      this.$nextTick(()=>{
        bus.$emit("countChange")
        this._initScroll()
      })
      
    },
    components: {
      'v-icon':icon,
      shopcart,
      cartcontrol,
      food
    },
    computed: {
      currentIndex(){
        let y = Math.abs(this.position.y-10)
        let index = this.listHeight.findIndex((val,index,arr)=>{
          return val>y
        })
        return index-1
      }
    },
    methods: {
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menu, {
          tap:true
        })
        this.foodsListScroll = new BScroll(this.$refs.foodsList, {
          probeType:3,
          click:true
        })
        setTimeout(()=> {
          this.menuScroll.refresh()
          this.foodsListScroll.refresh()
          this.computeHeight()
        }, 200);
        this.scrollEvent()
      },
      computeHeight(){
        let foodsDetail = this.$refs.foodsList.getElementsByClassName('foodsDetail')
        this.listHeight.push(0)
        for(let i=0;i<foodsDetail.length;i++){
          this.listHeight.push(foodsDetail[i].clientHeight+this.listHeight[i])
        }
      },
      scrollEvent(){
        this.foodsListScroll.on('scroll',(pos)=>{
          this.position.y = pos.y
        })
        this.$refs.menu.addEventListener('tap',(event)=>{
          this.skipEle(event)
        })
      },
      skipEle(){
        // 先判断是第几个
        // 好像与HTML高耦合了，但是现在只管写过去
        let dom = event.path[1]
        let leftList = this.$refs.menu.childNodes[0].childNodes
        let index = -1
        for(let i=0,item;item=leftList[i];i++){
          if(Object.is(item,dom)){
            index = i
            break;
          }
        }
        this.foodsListScroll.scrollTo(0,-(this.listHeight[index]),300)
      },
      clickGoodsItems(details,event){
        if(event.target.nodeName ==='I'){
          return
        }
        this.foodDetail = details
        this.$nextTick(function(){
          this.$refs.foodEle.toggleShow()
        })
      }
    },
  }
</script>

<style lang="scss" scope>
  @import '../../common/css/custom.scss';
  .goods{
    display: flex;
    flex-direction: column;
    flex: 1;
  }
  .goods-show{
    background: #f3f5f7;
    display: flex;
    flex:1;
    overflow: hidden;
    .menu{
      min-height: 100px;
      order:0;
      flex:1 0 (160rem/75);
      overflow: hidden;
      .typeList{
        height: calc(100% + 200px);
        .active{
          background: rgb(255,255,255);
          hr{
            display: none;
          }
        }
        li{
          display: table;
          width: 100%;
          height:(108rem/75);
          min-height: 108px;
          font-size: 24px;
          font-weight: 200;
          line-height: 28px;
          position: relative;
          // 标注图颜色标错了
          // color: rgb(240,20,20);
          .cutline{
            position: absolute;
            left: 24px;
            margin: 0;
            width:calc(100% - 48px);
            // min-width: 112px;
            height: 1px;
            background-color: rgba(7,17,27,0.1);
            border:none;
          }
          .text{
            display: table-cell;
            padding: 0 (24rem/75);
            text-align: center;
            vertical-align: middle;
          }
        }
      }
    }
    .foodsList{
      order:1;
      flex:1 0 (590rem/75);
      overflow: hidden;
      .foodsDetail{
        color: rgb(147, 153, 159);
        h5{
          padding-left:28px; 
          line-height: 52px;
          position: relative;
          hr{
            border: none;
            background-color: #d9dde1;
            width: 3px;
            height: 52px;
            position: absolute;
            left: 0;
            margin: 0;
          }
        }
        .goodsItem{
          background-color: #fff;
          display: flex;
          position: relative;
          padding: 36px;
          padding-bottom: 0;
          .cutline{
            width:518px;
            height: 1px;
            margin: 0;
            background-color: rgba(7,17,27,0.1);
            border: none;
            position: absolute;
            top: 0;
          }
          .info{
            padding-left: 20px;
            position: relative;
            h4{
              color:rgb(7,17,27);
              margin-top: 4px;
            }
            p{
              font-size: 20px;
              line-height: 20px;
              margin-top: 16px;
              &.price{
                display: block;
                position: relative;
                margin-bottom: 36px;
              }
              .rating{
                margin-left: 24px;
              }
              .newPrice{
                // 标注错了
                color: red;
                font-size: 28px;
                .symbol{
                  font-size: 20px;
                }
              }
              .oldPrice{
                text-decoration: line-through;
                padding-left: 16px;
              }
            }
          }
          .cartcontrol-wrapper{
              position: absolute;
              right: 36px;
              bottom: 23px;
          }
        }
        
      }
    }
  }
</style>