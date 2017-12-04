<template>
  <div class="star">
      <span v-for='(classItem,index) in classItems' :class='classItem' :key="index"></span>
  </div>
</template>

<script  type="text/ecmascript6">
    export default {
        props:{
            score:{
                type:Number
            },
            amount:{
                type:Number,
                default:5
            },
            size:{
                type:Number
            }
        },
        computed: {
            classItems(){
                let classItems = [],
                    score = Math.round(this.score),
                    size = this.size
                if(this.score>this.amount){
                    console.log('数值不对')
                }
                for(let i=0;i<score;i++){
                    classItems.push('on-'+size)
                }
                if(score>this.score){
                    classItems.pop()
                    classItems.push('half-'+size)
                }
                while(classItems.length!==this.amount){
                    classItems.push('off-'+size)
                }
                return classItems
            }
        }
    } 
</script>

<style lang="scss" scope>
    @import '../../common/css/mixin.scss';
    .star{
        display: flex;
    }
    span{
        flex:1;
        background-repeat: no-repeat;
    }
    span[class*='24']{
        width: 24px;
        height: 24px;
    }
    span[class*='36']{
        width: 36px;
        height: 36px;
    }
    span[class*='48']{
        width: 48px;
        height: 48px;
    }
    .on-24{
        @include bg-img('./star24_on');
    }
    .half-24{
        @include bg-img('./star24_half');
    }
    .off-24{
        @include bg-img('./star24_off');
    }
    .on-36{
        @include bg-img('./star36_on');
    }
    .half-36{
        @include bg-img('./star36_half');
    }
    .off-36{
        @include bg-img('./star36_off');
    }
    .on-48{
        @include bg-img('./star48_on');
    }
    .half-48{
        @include bg-img('./star48_half');
    }
    .off-48{
        @include bg-img('./star48_off');
    }
</style>