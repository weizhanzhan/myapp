<template>
  <div class="shopchart">
    <div class="scontent">
       <div class="chartleft" @click="pupopShow">
           <div class="logowrapper">
             <div :class="{'isshop':Allcount>0,'slogo':Allcount<1}">
                <img width="24" class="simgs" height="24" :src="shopicon.pic" />
                <div class="totalCount">
                   <span class="counttext">{{Allcount}}</span>
                 </div>
             </div>
           </div>
           <div :class="{'cpriceN':true||Allcount,'cpriceY':Allcount>0} ">￥{{allPrice}}</div>
           <div class="des">另需配送费{{deliveryPrice}} 元 </div>
       </div>
       
       <div class="chartright" :style="[allPrice>=minPrice?{background:bk,color:'#fff'}:'']" @click="pshow()">
          <div class="pay" >{{deliverys}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    props:[
        'deliveryPrice','minPrice'
    ],
    data(){
        return{
            shopsvg:{pic: require('./img/shopping_cart.svg'),text:'shopsvg'},
            shopicon:{pic: require('./img/cart.png'),text:'shopicon'},
            price:10,
            bk:'#1296db',
            popshow:false,
            balls:[
                {
                    show:false
                },
                {
                    show:false
                },
                {
                    show:false
                },
                {
                    show:false
                },
                {
                    show:false
                },
            ],
            dropBalls:[],
            
        }
    },
    created(){
    },
    methods:{
        pupopShow(){
            if(this.popShopList.length!=0){
                if(this.popupshow){ 
                    this.$store.commit('setPopupShow',false)
                }
                else{
                    this.$store.commit('setPopupShow',true)
                }
            }
            
        },
        pshow(){
            if(this.allPrice>=this.minPrice){
                this.popshow=!this.popupshow
                var obj={pops:this.popshow,popprice:this.allPrice}
                this.$emit('jspop', obj)
            }
        }
    },
    computed:{
        drop(){
            return this.$store.state.adddom
        },
        num(){
            return this.$store.state.orderFoods.length
        },
         popupshow(){            
             return this.$store.state.popupShow
        },
        popShopList(){
            return this.$store.state.selectFoods
        },
        Allcount(){
            if(this.popShopList.length==0){
                return 0
            }
            else{
                var allcount=this.popShopList.reduce((prcieSum,food)=>{
                    return prcieSum+food.count
                },0)
                return allcount
            }

        },
        allPrice(){
            var allprice=0
            this.popShopList.forEach(listfood=>{
                     allprice+=listfood.count*listfood.price
            })
            return allprice
        },
        deliverys(){
            if(this.minPrice-this.allPrice>0)
               return `还差${this.minPrice-this.allPrice}元起送`
            else
               return '结算'
           // return this.minPrice-this.allPrice
        }
       
    },
    watch:{
        drop(val){
            for(let i=0;i<this.balls.length;i++){
                let ball=this.balls[i]
                if(!ball.show){
                    ball.show=true;
                    ball.val=val;
                    this.dropBalls.push(ball)
                    return
                }
            }
        },
        popShopList(val){
            if(val.length==0)
               this.$store.commit('setPopupShow',false)
        }
    },
     transitions:{
        drop:{
          beforeEnter(el){
            let count=this.balls.length
            while(count--){
                let ball=this.balls[count]
                if(ball.show){
                    let rect=ball.el.getBoundingClientRect();
                    let x=rect.left-32
                    let y=-(window.innerHeight-rect.top-22)
                    el.style.display=""
                    el.style.webkitTransform=`translate3d(0,${y}px,0)`
                    el.style.transform=`translate3d(0,${y}px,0)`
                    let inner=el.getElementsByClassName('inner-hook')[0]
                    inner.style.webkitTransform=`translate3d(${x}px,0,0)`
                    el.style.transform=`translate3d(${x}px,0,0)`
                }
            }
          },
          enter(el){
             /* eslint-disable no-unused-vars*/
             let rf=el.offestHeight
             this.$nextTick(()=>{
                 el.style.webkitTransform='translate3d(0,0,0)'
                    el.style.transform='translate3d(0,0,0)'
                    let inner=el.getElementsByClassName('inner-hook')[0]
                    inner.style.webkitTransform='translate3d(0,0,0)'
                    el.style.transform='translate3d(0,0,0)'
             })
          },
          afterEnter(el){
             let ball=this.dropBalls.shift();
             if(ball){
                 ball.show=false
                 el.style.display='none'
             }
          }
        }
      },
    updated(){
    }

}
</script>

<style>
.shopchart{
    left: 0;
    bottom: 0;
    position: fixed;
    width: 100%;
    height:46px;
    /* background: #141d27; */
    z-index: 50;
    bottom: 0;
    font-size: 14px;
    color: hsla(0,0%,100%,.4);

}
.scontent{
      display: flex;
    width: 100%;
    height: 48px;
    font-size: 0;
    background-color: #141d27;
}
.chartleft{
  flex: 1
}
.chartright{
    flex: 0 0 105px;
    width: 105px;
    text-align: center;
    background:#2b333b
}
.pay{
    height: 48px;
    line-height: 48px;
    width: 100%;
    font-size: 12px;
    font-weight: 700;
}
.logowrapper{
    display:inline-block;
    position: relative;
    margin: 0 12px;
    top:-10px;
    height:56px;
    width:56px;
    padding: 6px;
    border-radius: 50%;
    box-sizing: border-box;
    vertical-align: top;
    background: #141d27;
    text-align: center
}
.slogo{
    width: 100%;
    height: 100%;
    border-radius: 50%;
    text-align: center;
    background: #2b343c
}
.isshop{
    background: #1296db;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    text-align: center;
}
.simgs{
    color: #2b343c;
    margin-top:10px
}
.cpriceN{
    display:inline-block;
    color:hsla(0,0%,100%,.4);
    margin-top: 10px;
    font-size: 16px;
    padding-right: 12px;
    font-weight: 700;
    line-height: 24px;
    border-right: 1px solid hsla(0,0%,100%,.1);
}
.cpriceY{
    display:inline-block;
    color:#fff;
    margin-top: 10px;
    font-size: 16px;
    padding-right: 12px;
    font-weight: 700;
    line-height: 24px;
    border-right: 1px solid hsla(0,0%,100%,.1);
}
.des{
    display: inline-block;
        font-size: 10px;
    color: hsla(0,0%,100%,.4);
    margin: 12px 0 0 12px;
}
.totalCount{
    position: absolute;
    right: 0;
    top:0;
    background: red;
    bottom: 30px;
    margin-left:30px;
    height: 16px;
    width: 24px;
    border-radius: 12px 12px;
    line-height: 16px;
    font-size: 9px;
    font-weight: 700;
    color: #fff;   
}
.counttext{
    /* position: fixed;
    margin-bottom: 20px */
}
.ball-container{
    display: flex;
    left: 32px;
    bottom: 22px;
    z-index: 200;  
    position: absolute;
}
 .drop-trasition{
     transition: all 0.4s
 }
 .inner{
     width: 16px;
     height: 16px;
     border-radius: 50%;
     background: rgb(0,160,220);
     transition:all 0.4s
 }
 .deliver{
     /* flex: 0 0 105px;
    width: 100%;
    text-align: center;
     background: rgb(0,160,220); */
 }
</style>
