<template>
<div class="shopcart">
    <div class="content">
        <div class="shop-icon-wrapper">
                <div class="shop-icon" :class="{'light':totalCount>0}" @click="toggleShopList">
                <i class="icon-shopping_cart"></i>
                <div v-show="totalCount>0" class="count">{{totalCount}}</div>
            </div>
        </div>
        <div class='detail'>
            <p class="goodsPrice" :class="{'light':totalPrice>0}">￥{{totalPrice}}</p>
            <hr class="cutLine">
            <p class="freight">另需配送费￥{{deliveryPrice}}元</p>
        </div>
    </div>
    <div class="submit" :class="{'light':totalPrice>=minPrice}">
        {{submitPrice}}
    </div>
    <div class="shopcart-list" v-show="shopShow">
        <div class="header">
            <p>购物车</p>
            <p class="clear" @click="clearList">清空</p>
        </div>
        <div class="shopList-wrapper" ref="listWrapper">
            <ul class="list-detail" ref="listDetail">
                <li v-for="(item,index) in this.list" :key="index" >
                    <p>{{item.name}}</p>
                    <p class="price"><span>￥</span>{{item.price}}</p>
                    <div class="cartcontrol-wrapper">
                        <cartcontrol :food="item"></cartcontrol>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</div>
</template>

<script type="text/ecmascript6">
    import icon from '../icon/icon'
    import BScroll from 'better-scroll'
    import cartcontrol from '../cartcontrol/cartcontrol'
    import bus from '../eventBus'
    

    export default{
        data(){
            return {
                list:[],
                shopShow:false
            }
        },  
        components: {
            'v-icon':icon,
            cartcontrol
        },
        props: {
            goods:{
                default:{}
            },
            deliveryPrice:{
                type:Number
            },
            minPrice:{
                type:Number
            }
        },
        computed: {
            totalCount(){
                let count = 0
                this.list.forEach((ele)=>{
                    count += ele.count
                })
                return count
            },
            totalPrice(){
                let price = 0
                this.list.forEach(function(ele) {
                    price+=ele.price*ele.count
                });
                return price
            },
            submitPrice(){
                let minPrice = this.minPrice
                if(this.totalPrice===0){
                    return `￥${minPrice}起送`
                }else if(this.totalPrice<this.minPrice){
                    let price = this.minPrice-this.totalPrice
                    return `还差￥${price}起送`
                }else if(this.totalPrice>=this.minPrice){
                    return `去结算`
                }
            }
        },
        methods: {
            _initScroll(){
                this.shopListScroll = new BScroll(this.$refs.listWrapper, {
                    probeType:3,
                    click:true
                })
                setTimeout(()=> {
                    this.shopListScroll.refresh()
                }, 1000);
                // this.scrollEvent()
            },
            screenList(){
                let itemList = [],
                    foodList = []
                for (let obj of this.goods) {
                    itemList =  obj.foods.filter((item)=>{
                       if(item.count>0){
                           return true
                       }
                    })
                    foodList.push(...itemList)
                }
                this.list = foodList
            },
            toggleShopList(){
                this.shopShow = !this.shopShow
                if(this.shopListScroll=== undefined){
                    this.$nextTick(()=>{
                        this._initScroll()
                    })
                }else{
                    this.shopListScroll.refresh()
                }
            },
            clearList(){
                this.list.forEach(function(item) {
                    item.count = 0
                })
                this.list = []
            },
            testListHeight(){
                let listDetail = this.$refs.listDetail
                if(listDetail!==undefined){
                    if(listDetail.clientHeight!==undefined && listDetail.clientHeight>=350){
                        return true
                    }else{
                        return false
                    }
                }
            }
        },
        mounted: function(){
            bus.$on('countChange',(str)=>{
                if(Array.isArray(this.goods)){
                    this.screenList()
                    if(this.testListHeight()){
                        this.$nextTick(()=>{
                            this._initScroll()
                        })
                    }
                }
            })
        }
    }
</script>

<style lang="scss" scope>
    .shopcart{
        flex:0 0 (96rem/75);
        display: flex;
        background-color: #141d27;
        color: rgba(255,255,255,0.4);
        position: relative;
        z-index: 100;
        .content{
            flex:1;
            display: flex;
            .shop-icon-wrapper{
                position: absolute ;
                bottom: 0;
                left: (24rem/75);
                height: (116rem/75);
                width:  (116rem/75);
                border-radius:  (58rem/75);
                background-color: #141d27;
                z-index: 100;
                .shop-icon{
                    height:(88rem/75);
                    width: (88rem/75);
                    background-color: rgba(255,255,255,0.2);
                    border-radius: (88rem/75);
                    margin: (12rem/75) auto (16rem/75);
                    font-size: 48px;
                    line-height: (88rem/75);
                    text-align: center;
                    &.light{
                        background: rgb(0,160,220);
                        color: #fff;
                    }
                    .count{
                        display: inline-block;
                        position: absolute;
                        top: 0;
                        right: 0;
                        width: (48rem/75);
                        font-size: 18px;
                        font-weight: 700;
                        color: rgba(255, 255, 255, 1);
                        line-height: 32px;
                        background: rgb(240,20,20);
                        border-radius: 16px;
                    }
                }
            }
            .detail{
                display: flex;
                margin-left: 160px;
                align-items:center;
                p{
                    font-size: 32px;
                    font-weight: 700;
                    line-height: (48rem/75);
                }
                .goodsPrice{
                    &.light{
                        color: rgb(255,255,255);
                    }
                }
                .freight{
                    font-size: 24px;
                    font-weight: 100;
                }
                .cutLine{
                    border: none;
                    width: 2px;
                    height: (48rem/75);
                    background: rgba(255, 255, 255, 0.1);
                    margin:0 24px;
                }
            }
        }
        .submit{
            flex:0 0 (210rem/75);
            font-size: 24px;
            line-height: 48px;
            display: flex;
            align-items: center;
            justify-content: center;
            &.light{
                background-color: rgb(0,180,60);
                color:#fff;
            }
        }
        .shopcart-list{
            display: block;
            position: absolute;
            bottom: (96rem/75);
            width: 100vw;
            overflow: hidden;
            p{
                font-size: 28px;
                color:rgb(7,17,27);
            }
            .header{
                height: (80rem/75);
                background-color: #f3f5f7;
                display: flex;
                justify-content:space-between;
                padding-left: 36px;
                border-bottom: 2px solid rgba(7, 17, 27, 0.1);
                z-index: 90;
                p{
                    font-weight: 200;
                    line-height: (80rem/75);
                }
                .clear{
                    font-size: 24px;
                    font-weight: normal;
                    color: rgb(0,160,220);
                    padding: 0  36px;
                }

            }
            .shopList-wrapper{
                max-height: 6rem;
                background: #fff;
            }
            .list-detail{
                z-index: 50;
                li{
                    box-sizing: border-box;
                    height: (96rem/75);
                    background: #fff;
                    position: relative;
                    border-left:36px solid #fff;
                    border-right: 36px solid #fff;
                    border-bottom: 1px solid #aaa;
                    p{
                        display: inline;
                        line-height: (48rem/75);
                        vertical-align: middle;
                    }
                    .price{
                        color: rgb(240,20,20);
                        line-height: 48px;
                        span{
                            font-size: 20px;
                            font-weight: normal;
                        }
                    }
                    .cartcontrol-wrapper{
                        position: absolute;
                        right: 36px;
                        bottom: 23px;
                        // 
                        .count{
                            font-size: 20px;
                            color: rgb(47,153,159);
                            line-height: 48px;
                            display: inline-block;
                            vertical-align: bottom;
                            width: 48px;
                            text-align: center;
                        }
                    }
                }
            }
            
        }
    }
</style>