<template>
    <div class="food" v-show="showState" ref="food">
        <div class="food-content" >
            <div class="info-basic">
                <div class="back">
                    <i class="icon-arrow_lift" @click="toggleShow"></i>
                </div>
                <img :src="detail.image" :alt="detail.name">
                <div class="text">
                    <p class="name">{{detail.name}}</p>
                    <p>
                    <span class="sales">月售{{detail.sellCount}}份</span>
                    <span class="rating">好评率100%</span>
                    </p>
                    <p class="price">￥{{detail.price}}
                        <span class="oldPrice" v-if="detail.oldPrice">￥{{detail.oldPrice}}</span>
                    </p>
                    <div class="ShopCart-wrapper">
                            <span @click="addCount(detail)" v-show="detail.count===0||detail.count===undefined">加入购物车</span>
                            <cartcontrol :food="detail" v-show="detail.count>0" ref="cartcontrol"></cartcontrol>
                    </div>
                </div>
            </div>
            <div class="info-intro" v-if="detail.info">
                <p class="intro-title">商品介绍</p>
                <p class="intro-detail">{{detail.info}}</p>
            </div>
            <ratingsDetails :ratings="detail.ratings"></ratingsDetails>
        </div>
    </div>
</template>

<script type="text/ecmascript6">
    import BScroll from 'better-scroll'
    import ratingsDetails from '../ratingsDetails/ratingsDetails'
    import cartcontrol from '../cartcontrol/cartcontrol'

    export default{
        components: {
            ratingsDetails,
            cartcontrol
        },
        data (){
            return {
                foodScroll:undefined,
                showState:false,
                imgSrc:undefined
            }
        },
        props: {
            detail:{
                type:Object
            }
        },
        methods: {
            _initScroll(){
                if(!this.foodScroll){
                    this.foodScroll = new BScroll(this.$refs.food, {
                        probeType:3,
                        click:true
                    })
                    setTimeout(()=> {
                        this.foodScroll.refresh()
                     }, 500);
                }
                
                // this.scrollEvent()
            },
            toggleShow(){
                this.$nextTick(function () {
                    this._initScroll()
                    this.showState = !this.showState
                })
            },
            addCount(obj){
                this.$set(obj,'count',1)
                this.$refs.cartcontrol.countChange()
            }
        },
        // created () {
        //     this._initScroll()
        // }
    }
    
</script>

<style lang="scss" scope>
    .food{
        width:100vw;
        height: 100vh;
        position: fixed;
        left: 0;
        top: 0;
        background: #f3f5f7;
        div[class|="info"]{
            background: #fff;
        }
        .back{
            position: absolute;
            left: 0.5rem;
            top:0.5rem;
            padding: 0.1rem;
            .icon-arrow_lift{
                color:#fff;
                font-size: (30rem/75);
            }
        }
        .food-content{
            padding-bottom: 2rem;
        }
        .text,.info-intro{
            padding:0 (36rem/75);
        }
        .info-basic{
            // font-size: 0;
            padding-bottom: 36px;
            border-bottom:2px solid rgba(7,17,27,0.1);
            img{
                display: block;
                width: 100vw;

            }
            p{
                font-size: (20rem/75);
                // 不知道什么原因必须重设p的字体才行，否则就有bug，奇怪
            }
            .text{
                position: relative;
            }
            .name{
                font-size: (28rem/75);
                font-weight: 700;
                color:rgb(7,17,27);
                line-height: 28px;
                margin-top: 36px;
                margin-bottom: 16px;
            }
            .sales,.rating{
                color: rgb(147,153,159);
                line-height: 20px;
                display: inline;
            }
            .sales{
                margin-right: 24px;
            }
            .price{
                line-height: 48px;
                font-weight: 700;
                color: rgb(240,20,20);
                font-size: (28rem/75);
                margin-top: 36px;
                .oldPrice{
                    font-size: (20rem/75);
                    font-weight: 700;
                    color: rgb(147,153,159);
                    margin-left: 10px;
                    text-decoration: line-through;
                }
            }
            .ShopCart-wrapper{
                    position: absolute;
                    right: (36rem/75);
                    bottom: 0;
                    span{
                        box-sizing: border-box;
                        display: inline-block;
                        height: (48rem/75);
                        width: (148rem/75);
                        color: rgb(255,255,255);
                        background-color: rgb(0,160,220);
                        font-size: (20rem/75);
                        line-height: (24rem/75);
                        padding:(12rem/75) (24rem/75);
                        border-radius: (24rem/75);
                    }
                }
        }
        .info-intro{
            margin-top:32px;
            margin-bottom: 32px;
            padding-top:36px;
            border: 1px solid rgba(7,17,27,0.1);
            border-left: none;
            border-right: none;
            .intro-title{
                font-size: (28rem/75);
                font-weight: 700;
                color:rgb(7,17,27);
                line-height: 28px;
                margin-bottom: 12px;
            }
            .intro-detail{
                font-size: (24rem/75);
                font-weight: 200;
                line-height: (48rem/75);
                color: rgb(77,85,93);
                padding: 16px;
                padding-bottom:36px;
            }
        }
    }
</style>