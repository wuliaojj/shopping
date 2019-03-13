<template>
    <div class="page">
        <div class="search-bar">
            <van-row>
                <van-col span="3">
                    <img :src="locationIcon" alt="" width="80%" class="location-icon">
                </van-col>
                <van-col span="16">
                    <input type="text" class="search-input">
                </van-col>
                <van-col span="5">
                    <van-button size="mini">查找</van-button>
                </van-col>
            </van-row>
        </div>

        <!-- swiper -->  
        <!-- autoplay自动轮播 -->
        <div class="swiper-area">
            <van-swipe :autoplay="2000">
                <van-swipe-item v-for="(banner,index) in bannerPicArray" :key="index">
                    <img v-lazy="banner.image" width="100%"/>
                </van-swipe-item>
            </van-swipe>
        </div>

        <!-- typebar -->

        <div class="type-bar">
            <div  v-for="(cate,index) in category" :key="index" >
                <img v-lazy="cate.image" width="90%" />
                <span>{{cate.mallCategoryName}}</span> 
            </div>
        </div>

        <!-- banner -->
        <div class="ad-banner">
            <img v-lazy="adBanner.PICTURE_ADDRESS" width="100%">
        </div>


        <!--Recommend goods area-->
        <div class="recommend-area">
            <div class="recommend-title">
                商品推荐
            </div>
            <div class="recommend-body">
                <!--swiper-->
                <swiper :options="swiperOption">
                   <swiper-slide v-for="(item,index) in recommendGoods " :key="index" >
                       <div class="recommend-item">
                           <img :src="item.image" width="80%">
                           <div>{{item.goodsName}}</div>
                           <div>￥{{item.price | moneyFilter}} (￥{{item.mallPrice | moneyFilter}})</div>
                       </div>
                   </swiper-slide> 
                </swiper>
            </div>
        </div>

    
        <!-- floor one area
        <div class="floor">
            <div class="floor-title">
                商品推荐
            </div>
            <div class="floor-anomaly">
                <div class="floor-one"><img :src="floor1_0.image" width="100%" /></div>
                <div>
                    <div class="floor-two"><img :src="floor1_1.image" width="100%" /></div>
                    <div><img :src="floor1_2.image" width="100%" /></div>
                </div>
            </div>
            <div class="floor-rule">
                <div v-for="(item ,index) in floor1.slice(3)" :key="index">
                    <img :src="item.image" width="100%"/>
                </div>
            </div>
        
        </div> -->
        <floorComponent :floorData="floor1" :floorTitle="floorName.floor1"></floorComponent>
        <floorComponent :floorData="floor2" :floorTitle="floorName.floor2"></floorComponent>
        <floorComponent :floorData="floor3" :floorTitle="floorName.floor3"></floorComponent>


        <!--Hot Area-->
        <div class="hot-area">
            <div class="hot-title">热卖商品</div>
            <div class="hot-goods">
                <van-row gutter="20" style="margin:0;">
                    <van-col span="12" v-for="(item,index) in hotGoods" :key="index">
                        <goods-info :goodsImage="item.image" :goodsName="item.name" :goodsPrice="item.price"></goods-info>
                    </van-col>
                </van-row>
            </div>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
import axios from 'axios'
import {swiper , swiperSlide} from 'vue-awesome-swiper'
import 'swiper/dist/css/swiper.css'
import floorComponent from '../component/floorComponent'
import goodsInfo from '../component/goodsInfoComponent'
import { toMoney } from '../../filter/moneyFilter'
import url from '../../serviceAPI.config'
export default {
    data() {
        return {
            swiperOption:{
                slidesPerView:3
            },
            msg:'123',
            locationIcon: require('../../assets/images/location.png'),
            bannerPicArray:[],
            category:[],
            adBanner:'',
            recommendGoods:[],
            floor1:[],
            floor2:[],
            floor3:[],
            // floor1_0:{},
            // floor1_1:{},
            // floor1_2:{},
            floorName:[],
            hotGoods:[] //热卖商品
        }
    },
    components: {
        swiper,
        swiperSlide,
        floorComponent,
        goodsInfo
    },
    filters:{
        moneyFilter(money){
            return toMoney(money)
        } 
    },
    // http://jspang.com/DemoApi/typeGoods.php
    created(){
        axios({
            url: url.getShoppingMallInfo,
            method: 'get',
        })
        .then(response => {
            console.log(response)
            if(response.status==200){
                this.bannerPicArray = response.data.data.slides //获得轮播图片
                this.category=response.data.data.category
                this.adBanner = response.data.data.advertesPicture //获得广告图片
                this.recommendGoods = response.data.data.recommend
                this.floor1 = response.data.data.floor1              //楼层1数据
                // this.floor1_0 =this.floor1[0]
                // this.floor1_1 =this.floor1[1]
                // this.floor1_2 =this.floor1[2]
                this.floor2 = response.data.data.floor2 
                this.floor3 = response.data.data.floor3
                this.floorName = response.data.data.floorName
                this.hotGoods = response.data.data.hotGoods  
            }
        })
        .catch((error) => {     
        })
    },
}
</script>

<style lang='less'>
.search-bar{
    height: 2.2rem;
    background-color: #e5017d;
    line-height:2.2rem;
    overflow: hidden;
    .search-input{
        width:100%;
        height: 1.3rem;
        border-top:0px;
        border-left:0px;
        border-right:0px;
        border-bottom: 1px solid 1px !important ;
        background-color: #e5017d;
        color:#fff;
    }
    .location-icon{
        padding-top: .2rem;
        padding-left: .3rem;
    }
}
.swiper-area{
    width: 20rem;
    clear: both;
}
.type-bar{
    background-color: #fff;
    margin:0 .3rem .3rem .3rem;
    border-radius: .3rem;
    font-size:14px;
    display: flex;
    flex-direction:row;
    flex-wrap:nowrap;
    div{
        padding:.3rem;
        font-size:12px;
        text-align: center;
        flex:1;
    }
}

.recommend-area{
    background-color: #fff;
    margin-top: .3rem;
    .recommend-title{
    border-bottom:1px solid #eee;
    font-size:14px;
    padding:.2rem;
    color:#e5017d;
    }
    .recommend-body{
       border-bottom: 1px solid #eee;
       .recommend-item{
        width:99%;
        border-right:1px solid #eee;
        font-size:12px;
        text-align: center;
        }
   }
}


// .floor{
//     .floor-title{
//         border-bottom:1px solid #eee;
//         font-size:14px;
//         padding:.2rem;
//         text-align: center
//     }
//     .floor-anomaly{
//         display: flex;
//         flex-direction:row;
//         background-color: #fff;
//         border-bottom:1px solid #ddd;
//         div{
//                 width:10rem;
//                 box-sizing: border-box;
//                 -webkit-box-sizing: border-box;
//                 .floor-two{
//                     border-bottom:1px solid #ddd;
//                 }
//             }
//         .floor-one{
//             border-right:1px solid #ddd;
//         }
//     }
//     .floor-rule{
//         display: flex;
//         flex-direction: row;
//         flex-wrap:wrap;
//         background-color: #fff;
//         div{
//             -webkit-box-sizing: border-box;
//             box-sizing: border-box;
//             width:10rem;
//             border-bottom:1px solid #ddd;
//             &:nth-child(odd){
//                 border-right: 1px solid #ddd;
//             }
//         }
//     }
// }
 
.hot-area{
    text-align: center;
    font-size:14px;
    height: 1.8rem;
    line-height:1.8rem;
}
  


</style>
