<template>
  <span class="icon" :class="form"></span>
</template>

<script  type="text/ecmascript6">
    export default{
        props: {
            form:{
                type:String
            }
        }
    }
</script>

<style lang="scss" scope>
    @import '../../common/css/mixin';
    .icon{
      display: inline-block;
      background-size: cover;
      vertical-align: text-bottom;
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
</style>