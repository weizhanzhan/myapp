<template>
<div>
  <div class="detail" v-show="detailShow">
      <div class="imgheader"><img class="imgdetail" :src="selectFoodDetail.image"/></div>
      <div class="dcontent">
          <div class="dname">{{selectFoodDetail.name}}</div>
          <div class="ddetail">
              <span class="dcount">月售{{selectFoodDetail.sellCount}}</span>
              <span class="drating">好评率{{selectFoodDetail.rating}}%</span>
          </div>
          <div class="dprice">
              <span class="dsprice">￥{{selectFoodDetail.price}}</span>
          </div>
          <div class="dcontrol">
              <div>
                  <transition name="food"> 
                   <control v-if="selectFoodDetail.count>0" :food="food" ></control>
                   <div  v-if="!selectFoodDetail.count" class="dshopchart" @click="buy(selectFoodDetail)">放入购物车</div>
                  </transition>
              </div>
          </div>
      </div>
      <div class="dhr"></div>
      <div class="ddesc">
          <h1 class="ddesc_title">商品介绍</h1>
          <div class="ddesc_content">{{selectFoodDetail.info}}</div>
      </div>
      <div class="dhr"></div>
      <div class="drat">
          <h1 class="drat_title">商品评价</h1>
          <div class="ratings_select">
              <div class="dtype">
                  <span class="block_positive" id="1" :class="{activeColor:isactive==ratAll.All.id}" @click="slect($event)">全部{{ratAll.All.num}}</span>
                  <span class="block_positive" id="2" :class="{activeColor:isactive==ratAll.likes.id}" @click="slect($event)">推荐{{ratAll.likes.num}}</span>
                  <span class="block_negative" id="3" :class="{activeColor:isactive==ratAll.dislikes.id}" @click="slect($event)">吐槽{{ratAll.dislikes.num}}</span>
              </div>
              <div class="switch" @click="switchs()"> 
                    <span class="iconspan" ><van-icon class="switch_icon"  name="checked" :style="[isSelect? {color:look}:'']" /></span>
                    <span>只看有内容的评价</span>
              </div>
          </div>
      </div>
      <div class="rating-wraper">
          <ul>
              <li class="rates" v-for="(rate,index) in typeRate" v-if="isSelect?rate.text!='':rate.text!=undefined" :key="'a'+index">
                  <div class="rate-name">
                     <span class="rate-name-title"> {{rate.username}}</span>
                     <img class="rate-img" :src="rate.avatar" />
                  </div>
                  <div class="rate-time">{{rate.rateTime}}</div>
                  <div class="rate-text">
                      <img :src="rate.rateType?down.pic:up.pic" class="iconclass"/>
                      {{rate.text}}
                  </div>
              </li>
          </ul>
      </div>
  </div>
 
  <div class="rate-back">
       <img :src="back.pic"  width="30" class="rate-back-img"  height="30" @click="goback" />
  </div>
</div>
</template>

<script>
import Control from '../control/control'
import Vue from 'vue'
export default {
    props:{
        selectFoodDetail:{
            type:Object
        }
    },
    created(){
        this.food=this.selectFoodDetail
    },
    data(){
        return{
          food:'',
          isactive:'',
          look:"green",
          isSelect:false,
          up:{pic: require('./img/up.png'),text:'up'},
          down:{pic: require('./img/down.png'),text:'down'},
          selectRate:'',
          backsvg:{pic: require('./img/backsvg.svg'),text:'返回svg'},
          back:{pic: require('./img/back.png'),text:'返回'},
        }
    },
    methods:{
        goback(){
            this.$store.commit('setDetail',false)
        },
        buy(val){
             Vue.set(val,'count',1)
             this.$store.commit("setOrder",val)          
        },
        slect(val){
            this.isactive=val.target.id           
            if(val.target.id==1)
               this.selectRate=''
            else if(val.target.id==2)
               this.selectRate=0
            else
               this.selectRate=1
            
        },
        switchs(){
            this.isSelect=!this.isSelect
        },
        rateType(val){
            if(val)
            return `<img :src="${this.up.pic}" />`
        }
    },
    components:{
        Control
    }
    ,
    computed:{
        detailShow(){
            return this.$store.state.detailShow 
        },
        ratAll(){
            var ratings=this.selectFoodDetail.ratings,
            rates={
                likes:{num:0,id:2},
                dislikes:{num:0,id:3},
                All:{num:0,id:1},}
            ratings.forEach(rating => {
                if(rating.rateType==0)
                   rates.likes.num++
                else{
                   rates.dislikes.num++
                }
            });
            rates.All.num=ratings.length
            return rates
        },
        typeRate(){
          let rates=this.selectFoodDetail.ratings
            return  rates.filter(rate=>{
                var b=JSON.stringify(rate.rateType)
                return b.match(this.selectRate)
            })  
        },
    }

}
</script>

<style>
.detail{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    bottom: 46px;
    transform: translateZ(0);
    background: #fff ;
    z-index: 2;
    overflow:scroll;
}
.imgheader{
    position: relative;
    width: 100%;
    height:0;
    padding-top: 100%;
}
.imgdetail{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
.dcontent{
    position: relative;
    padding: 18px;
}
.dname{
 line-height: 14px;
    margin-bottom: 8px;
    font-size: 14px;
    font-weight: 700;
    color: #07111b;
}
.ddetail{
        margin-bottom: 18px;
    line-height: 25px;
    height: 10px;
    font-size: 0;
}
.dcount{
    display: inline-block;
    font-size: 10px;
    color: #93999f;
}
.drating{
    display: inline-block;
    font-size: 10px;
    color: #93999f;
    margin-left: 12px;
}
.dprice{
    font-weight: 700;
    font-size: 14px;
    line-height: 24px;
    
}
.dsprice{
    font-size: 14px;
    color: #f01414;
}
.dcontrol{
    position:absolute;
    right: 12px;
    bottom: 24px;
}
.dshopchart{
    position: absolute;
    text-align: center;
    right: 0rem;
    bottom: 0.2rem;
    width: 8rem;
    height: 24px;
    line-height: 24px;
    padding:0 12px;
    box-sizing: border-box;
    border-radius: 12px;
    font-size: 10px;
    background: #ffd161;
    z-index: 10;

}
.dhr{
     width: 100%;
    height: 16px;
    background-color: #ebebeb
}
.ddesc{
    padding: 18px
}
.ddesc_title{
    line-height: 14px;
    margin-bottom: 6px;
    font-size: 14px;
    color: #07111b
}
.ddesc_content{
    padding: 0 8px;
    font-size: 12px;
    font-weight: 200;
    line-height: 24px;
    color: #4d555d;
}
.drat{
    padding: 18px
}
.drat_title{
    line-height: 14px;
    margin-bottom: 6px;
    font-size: 14px;
    color: #07111b
}
.dtype{
    padding: 18px 0;
    position: relative;
}
.block_positive{
   background-color: rgba(0,160,220,.2);
   padding: 8px 12px;
   margin-right: 8px;
    border-radius: 2px;
   font-size: 12px;
   line-height: 16px;
    color: #4d555d;
}
.block_negative{
    background-color: rgba(77,85,93,.2);
    padding: 8px 12px;
    margin-right: 8px;
    border-radius: 2px;
    font-size: 12px;
    line-height: 16px;
    color: #4d555d;
}
.activeColor{
    background:#00a0dc;
        color: #fff
    }
.switch{
    padding: 12px 18px;
    line-height: 24px;
    color: #93999f;
    border-bottom: 1px solid rgba(7,17,27,.1);
}
.switch_icon{
    
}
.rating-wraper{
    padding: 18px;
}
.rates{
    position:relative;
    padding: 18px 0;
    border-bottom: 1px solid rgba(7,17,27,.1);
}
.rate-name{
    position: absolute;
    top:16px;
    right: 0;
    line-height: 12px
}
.rate-name-title{
    display: inline-block;
    vertical-align: top;
    font-size: 10px;
    color: #93999f;
    margin-right: 6px;

}
.rate-img{
    position: relative;
    bottom:3px;
    width: 12px;
    height: 12px;
    border-radius: 50%
}
.rate-time{
    margin-bottom: 6px;
    line-height: 12px;
    font-size: 10px;
    color: #93999f;
}
.rate-text{
    line-height: 16px;
    font-size: 12px;
    color: #07111b;
}
.iconclass{
    position: relative;
    bottom:2px;
    line-height: 24px;
    margin-right: 4px;
    width: 12px;
}
.iconspan{
    position: relative;
    top:3px
} 
.rate-back{
     position: fixed;
     top: 25px;
     left: 20px;
     width: 35px;
     height: 35px;
     border-radius: 50%;
     background-color: rgba(0,0,0,.2);
     z-index: 40;
}
.rate-back-img{
    
}
 .food-enter-active, .food-leave-active{
  transition: opacity 0.5s;
}
.food-enter, .food-leave-to{
  opacity: 0;
}
</style>
