<template>
  <div class="shopcart">
      <div class="content">
          <div class="shop-icon-wrapper">
              <div class="shop-icon" :class="{'light':totalCount>0}">
                  <i class="icon-shopping_cart"></i>
                  <div v-if="totalCount>0" class="count">{{totalCount}}</div>
              </div>
          </div>
          <div class='detail'>
            <p class="goodsPrice" :class="{'light':totalPrice>0}">￥{{totalPrice}}</p>
            <hr class="cutLine">
            <p class="freight">另需配送费￥{{deliveryPrice}}元</p>
          </div>
      </div>
      <div class="submit" :class="{'light':totalPrice>minPrice}">
          {{submitPrice}}
      </div>
  </div>
</template>

<script type="text/ecmascript6">
    import icon from '../icon/icon'

    export default{
        components: {
            'v-icon':icon,
        },
        props: {
            list:{
                type:Array
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
                console.log(this.list)
                let price = 0
                this.list.forEach(function(ele) {
                    price+=ele.price*ele.count
                });
                return price
            },
            submitPrice(){
                if(this.totalPrice===0){
                    return `￥${minPrice}起送`
                }else if(this.totalPrice<this.minPrice){
                    let price = this.minPrice-this.totalPrice
                    return `还差￥${price}起送`
                }else if(this.totalPrice>=this.minPrice){
                    return `去结算`
                }
            }
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
        opacity: 0.9;
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
    }
</style>