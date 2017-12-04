<template>
  <div class="cartcontrol" ref="cartcontrol">
      <transition name="fade move">
      <div class="remove" v-if='food.count' @click="removeCount"><i class="icon-remove_circle_outline"></i></div>
      </transition>
      <transition name="fade">
      <p class="count" v-if='food.count'>{{food.count}}</p>
      </transition>
      <div class="add" @click="addCount"><i class="icon-add_circle" ></i></div>
      <!-- <transition-group name="throwY throwX">
          <div class="ball" v-for="(ball,index) in balls" :key="index" v-if="ball.show"></div>
      </transition-group> -->
  </div>
</template>

<script type="text/ecmascript6">
    import Vue from 'vue'
    import bus from '../eventBus'

    export default{
        data () {
            return {
                // balls:[
                //     {
                //         show:false
                //     },{
                //         show:false
                //     },{
                //         show:false
                //     },{
                //         show:false
                //     },{
                //         show:false
                //     },{
                //         show:false
                //     },{
                //         show:false
                //     },
                // ],
            }
        },
        props:{
            food:{
                type:Object
            }
        },
        computed: {
            
        },
        methods:{
            addCount(event){
                
                // if(!event._constructed){
                //     return
                // }
                if(!this.food.count){
                    // 不推荐修改props，那么有什么好的办法吗？或者这并不算是修改？单纯的增加？
                    this.$set(this.food,'count',1)
                }else{
                    this.food.count++
                }
                this.countChange()  
                // this.dropBall(event.target)
            },
            removeCount(event){
                // if(!event._constructed){
                //     return
                // }
                if(this.food.count>0){
                    this.food.count--
                }
                this.countChange()    
            },
            countChange(){
                bus.$emit("countChange")
            },
            // dropBall(dom){
            //     let index = 0
            //     for(let i=0,ele;ele=this.balls[i++];){
            //         if(!ele.show){
            //             index = i-1
            //             ele.show=true
            //             setTimeout(function() {
            //                 ele.show=false
            //             }, 100000);
            //             break
            //         }
            //     }
            //     let rect = dom.getBoundingClientRect()
            // }
        }
    }
</script>

<style lang="scss" scope>
    .cartcontrol{
        font-size: 0;
        height: 100%;
        div{
            display: inline-block;
            font-size: 48px;
            color: rgb(0, 160, 220);
        }
        .count{
            font-size: 20px;
            color: rgb(47,153,159);
            line-height: 48px;
            display: inline-block;
            vertical-align: bottom;
            width: 48px;
            text-align: center;
        }
        .fade-enter-active, .fade-leave-active {
            transition: opacity 1s;
        }
        .fade-enter, .fade-leave-to{
            opacity: 0;
        }
        .move-enter{
            transform: translate3d(48px,0,0);
        }
        .move-enter-to,{
            transform: rotate3D(0,0,1,180deg) translate3d(0,0,0);
        }
        
        .move-leave{
            transform:rotate3D(0,0,1,0deg) ;
        }
        .move-leave-to{
            transform:rotate3D(0,0,1,-180deg) translateX(-48px);
        }
        
        .move-enter-active, .move-leave-active {
            transition: all .5s linear;
        }
        .ball{
            width: 20px;
            height: 20px;
            border-radius: 20px;
            background: rgb(0,160,220);
            position: absolute;
            top:0;
            right:12px;
            z-index: 100;
        }
    }
</style>