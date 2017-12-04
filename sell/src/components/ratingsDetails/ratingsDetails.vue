    <template>
    <div class="rating">
        <div class="rating-select">
            <p class="rating-title">商品评价</p>
            <div class="select-type" @click="toggleSelectType($event)">
                <button>{{selectDesc.all}}</button>
                <button>{{selectDesc.pleased}}</button>
                <button>{{selectDesc.unpleased}}</button>
            </div>
            <p class="select-content" @click="toggleSelectContent($event)"><i :class="[OnlyContentState?'icon-active':'','icon-check_circle']"></i><span>只看有内容的评价</span></p> 
        </div>
        <ul class="rating-details">
            <li v-for="(item,index) in ratings" :key="index" v-if="onlyContentShow(item.text) && selectTypeShow(item.rateType)">
                <div class="ratings-info">
                    <p class="time"><span class="date">{{timeShift(item.rateTime)[0]}}</span>{{timeShift(item.rateTime)[1]}}</p>
                    <p class="user-info"><span class="name">{{item.username}}</span><img :src="item.avatar" alt="头像" class="avatar"></p>
                </div>
                <div class="rating-content">
                    <p><i :class="item.rateType===0?'icon-thumb_up':'icon-thumb_down'"></i><span class="rating-text">{{item.text}}</span></p>
                </div>
            </li>
        </ul>    
    </div>
</template>

<script type="text/ecmascript6">
const ALL=-1,
      PLEASED = 0,
      UNPLEASED = 1


export default {
  data () {
    return {
        OnlyContentState:this.onlyContent,
        selectTypeState:this.selectType
    };
  },
  props: {
      ratings:{
          type:Array
      },
      selectDesc:{
          type:Object,
          default(){
              return{
                  all:'全部',
                  pleased:'推荐',
                  unpleased:'吐槽'
              }
          }
      },
      selectType:{
          type:Number,
          default(){
              return ALL
          }
      },
      onlyContent:{
          type:Boolean,
          default(){
              return false
          }
      }
  },
  methods: {
      timeShift(UTCtime){
            let date = new Date(UTCtime)
            let timeStr = date.toTimeString(),
                dateStr = date.toLocaleDateString()
            // 两个都需要进行处理，与所需的样式请自己对比
            timeStr = timeStr.split(" ")[0]
            dateStr = dateStr.split("/").join("-")
            return [dateStr,timeStr]
      },
      toggleSelectContent(event){
          this.OnlyContentState = !this.OnlyContentState
          this.$emit('contentChange')
      },
      toggleSelectType(event){
          this.$emit('contentChange')
            let target = event.target,
                parentNode = target.parentNode,
                nodeList = [].filter.call(parentNode.childNodes,function(item) {
                    if(item.nodeName === 'BUTTON'){
                        return true
                    }
                })
            for(let i=0;i<nodeList.length;i++){
                if(target === nodeList[i]){
                    switch (i-1) {
                        case ALL:
                            this.selectTypeState = ALL
                            break
                        case PLEASED:
                            this.selectTypeState = PLEASED
                            break
                        case UNPLEASED:
                            this.selectTypeState = UNPLEASED
                        break
                        default:
                            break;
                    }
                    break;
                }
            }
      },
      onlyContentShow(text){
          let result = true
          if(this.OnlyContentState){
              if(text===''){
                  result = false
              }
          }
          return result
      },
      selectTypeShow(type){
          if(this.selectTypeState === ALL || type === this.selectTypeState){
              return true
          }else{
              return false
          }
      }
  }
}
</script>
<style lang="scss" scoped>
    .rating{
        background: #fff;
        border-top: 1px solid rgba(7,17,27,0.1);
        .rating-select{
            padding:36px;
            padding-bottom: 0;
            border-bottom: 3px solid rgba(7,17,27,0.1);
            .rating-title{
                font-size: (28rem/75);
                color: rgba(7,17,27,1);
                line-height: (28rem/75);
                margin-bottom: 36px;
            }
            .select-type{
                height: 100%;
                overflow: auto;
                border-bottom: 1px solid rgba(7,17,27,0.1);
                padding-bottom: 36px;
                button{
                    border:none;
                    float: left;
                    // 不清楚为什么父元素的高度为什么那么奇怪，button好像没什么多余的东西啊
                    margin-right:16px;
                    padding: (16rem/75) (24rem/75);
                    font-size: (28rem/75);
                    line-height: (32rem/75);
                    border-radius:(2rem/75);
                    color:rgb(77,85,93);
                    &:nth-of-type(1){
                        color:rgb(255,255,255);
                        background-color: rgb(0,160,220);
                    }
                    &:nth-of-type(2){
                        background-color: rgba(0,160,220,0.2);
                    }
                    &:nth-of-type(3){
                        background-color: rgba(77,85,93,0.2);;
                    }
                }
            }
            .select-content{
                font-size: (24rem/75);
                color:rgb(147,153,159);
                line-height: (48rem/75);
                margin: (24rem/75) 0;
                i{
                    font-size: (48rem/75);
                    margin-right: (8rem/75);
                    vertical-align: middle;
                }
                .icon-active{
                    color:rgb(0,160,220);
                }
                span{
                    vertical-align: middle;
                }
            }
        }
        .rating-details{
            padding:36px;
            padding-top:0px;
            li{
                margin-top: 32px;
            }
            .ratings-info{
                display: flex;
                justify-content: space-between;
                font-size: (20rem/75);
                line-height: (48rem/75);
                color:rgb(147,153,159);
                .time{
                    vertical-align: middle;
                    .date{
                        margin-right: (25rem/75);
                    }
                }
                .user-info{
                    .name{
                        vertical-align: middle;
                    }
                    img{
                        vertical-align: middle;
                        margin-left: (12rem/75);
                        width: (24rem/75);
                        height: (24rem/75);
                        border-radius: (24rem/75);
                    }
                }
            }
            .rating-content{
                p{
                    font-size: (24rem/75);
                    color: rgba(7,17,27,1);
                    line-height: (32rem/75);
                    margin:(12rem/75) 0 0 0;
                    padding-bottom: (32rem/75);
                    border-bottom: 1px solid rgba(7,17,27,0.1); 
                    i{
                        font-size: (24rem/75);
                        color:rgb(147,153,159);
                        line-height: (48rem/75);
                        margin-right: (8rem/75);
                    }
                    .icon-thumb_up{
                        color:rgb(0,160,220);
                    }
                }
            }
        }
    }
</style>