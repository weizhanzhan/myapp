<template>
  <div >
      <div class="title-bar">
          <span>附近商家</span>
      </div>
      <div v-for="(shopinfo,index) in shopList" :key="index" @click="$router.push('./infomation')" class="indexdiv" >
          <span class="sleft" >
               <div class="imgall">
                    <div class="imgpic"><img :src="shopinfo.pic_url" style="width:70px;" alt="" id=""></div>
                    <div class="imgicon"><img :src="shopinfo.poi_type_icon" style="width:30px;float:left" alt="" id=""></div>
                  </div>
          </span>
          <span class="sright" >
               <div><label>{{shopinfo.name}}</label></div>
                       <div class="middleinfo">
                         <div class="middleimg">
                            <img v-for="(num,index) in parseInt(shopinfo.wm_poi_score)" :key="index" :src="starurl.star[2].src" style="height:10px;padding-left:2px"/>
                            <img v-for="(num,index) in Math.ceil(shopinfo.wm_poi_score)-parseInt(shopinfo.wm_poi_score)" :key="index+'w'" style="height:10px;" :src="starurl.star[1].src"/>
                         </div>
                         <span>月售{{shopinfo.month_sale_num}}</span>
                         <span class="middletime">{{shopinfo.mt_delivery_time}}|{{shopinfo.distance}}</span>
                       </div>
                     <div class="middlezs">
                         <span class="deliver">{{shopinfo.min_price_tip}}</span>|
                         <span class="deliver">{{shopinfo.shipping_fee_tip}}</span>|
                         {{shopinfo.average_price_tip}}
                         </div>
                     <div class="shopinfo" v-for="(count,index) in shopinfo.discounts2" :key="index">
                         <img :src="count.icon_url" style="width:16px">
                         {{count.info}}
                    </div>
          </span>
      </div>
  </div>
</template>
<script>
import indexList from '../../static/index-list.json'
import star from '../../static/json/star.json'	
export default {
    name:'indexList',
    data(){
        return{
           shopList:[],
        }
    },
    created(){
        this.shopList=indexList.data.poilist
    },
    methods:{
        onClickLeft(){
            this.$router.back(-1)
        }
    },
    computed:{
        starurl(){
            return star
        }
    }
  
}
</script>
<style>

.imgall{
        position: relative;
        text-align: center;
        width: 90px;
        height: 93px;
        overflow: hidden;
    }
    .imgpic{
        /* width: 100px; */
        height: 100px;
    }
   .imgicon{
        position: absolute;
        top:0;
        left: 0;
        width:50px;
        height: 50px;
    }
    p[class="shopinfo"] {
    display: block;
    color: darkgrey;
    margin: 0;
    padding: 0
  
}
.indexdiv{
    /* height:160px;
    border-bottom:1px solid #ebebeb */
    margin-bottom: 5px;
    display: flex;
    flex-direction: row;
    padding: 10px 0;
    overflow: hidden;
    position: relative;
    border-bottom:1px solid #ebebeb
}
.sleft{
    /* width:20%;
    float:left;
    margin-top:20px */
    flex: 0 0 90px;
    width: 90px;
}
.sright{
    /* width:75%;
    float:right;
    margin-top:15px */
    flex: 1;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}
.title-bar{
    height:40px;
    line-height:40px;
    text-align:center
}
.title-bar span{
    display: inline-block;
    position: relative;
    font-weight: 700;
    color: #333;
}
.middleinfo{
   -webkit-box-flex: 1;
    -ms-flex: 1;
    flex: 1;
    font-size: 12px;
    color: #666;
    margin-top: 0px;
}
.middleimg{
    float:left;
    margin-top: -3px
}
.middlezs{
    -webkit-box-flex: 1;
    -ms-flex: 1;
    flex: 1;
    color: #666;
    font-size: 13px;
}
.shopinfo{
    -webkit-box-flex: 1;
    -ms-flex: 1;
    flex: 1;
    font-size: 13px;
    color: #b0b0b0;
}
.middletime{
    float: right;
    margin-right: 10px;
    position: relative;
    letter-spacing:2px
}
.deliver{
        margin-right: 10px;
    position: relative;
}
</style>
