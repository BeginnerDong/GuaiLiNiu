<template>
	<view :style="sb_show?'height:100%;overflow:hidden':''">
		<view class="backBox" @click="Router.back({route:{dalta:-1}})">
			<image src="../../static/images/back-icon.png" class="back"></image>
		</view>
		<!-- banner -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff" class="banner">
			<swiper-item v-for="(item,index) in mainData.bannerImg" :key="index">
				<image :src="item.url" ></image>
			</swiper-item>
		</swiper>
		
		<view class="content bg-white radius20-T p-r px-2">
			<view class="pb-4 bB-f5">
				<view class="flex pt-5">
					<view class="font-34">{{mainData.name?mainData.name:''}}</view>
					<view class="font-20 colorf p-r sq">
						<image src="../../static/images/home-icon5.png" class="sq-icon"></image>
						<view class="top-0 p-a t-indent10 line-h-md">社区店</view>
					</view>
				</view>
				<button open-type="share" class="shareSgin">
					<image src="../../static/images/course-icon2.png" class="fx-icon"></image>
					<view>分享</view>
				</button>
				<view class="font-22 line-h py-2 color9">
					门店面积 <text class="priceM pl-1">{{mainData.total_area?mainData.total_area:''}}</text> | 
					私教面积 <text class="priceM pl-1">{{mainData.private_area?mainData.private_area:''}}</text>
				</view>
				<view class="flex1">
					<view class="dz avoidOverflow2 font-26 color6">地址：{{mainData.address?mainData.address:''}}</view>
					<view class="flex4 font-20 color6">
						<image src="../../static/images/stores-details-icon.png" class="storeDz-icon mb-2"></image>
						<view>距离{{distance}}KM</view>
					</view>
				</view>
			</view>
			
			<view class="pb-4 bB-f5" v-if="mainData.active.length>0">
				<view class="font-w t-indent20 line-h pt-4 pb-3 tit">限时特惠</view>
				<image :src="mainData.active&&mainData.active[0]&&mainData.active[0].mainImg?mainData.active[0].mainImg[0].url:''" class="storeImg1" 
				@click="Router.navigateTo({route:{path:'/pages/limitedTime/limitedTime?id='+mainData.active[0].id}})"></image>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="flex1">
					<view class="font-w t-indent20 line-h pt-4 pb-3 tit">门店详情</view>
					<view class="color6 flex1" @click="isShow(2)">
						<view>查看全部</view>
						<image src="../../static/images/the-order-icon3.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="flex1 color6 font-24">
					<view class="flex4">
						<image src="../../static/images/stores-details-icon2.png" class="wh80 mb-2"></image>
						<view>无线网络</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores-details-icon3.png" class="wh80 mb-2"></image>
						<view>储物柜</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores-details-icon4.png" class="wh80 mb-2"></image>
						<view>更衣室</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores-details-icon5.png" class="wh80 mb-2"></image>
						<view>饮水机</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores-details-icon6.png" class="wh80 mb-2"></image>
						<view>人脸识别</view>
					</view>
				</view>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="flex1">
					<view class="font-w t-indent20 line-h pt-4 pb-3 tit">门店私教</view>
					<view class="color6 flex1" @click="Router.navigateTo({route:{path:'/pages/store-sijiao/store-sijiao?shop_no='+shopData.user_no}})">
						<view>查看全部</view>
						<image src="../../static/images/the-order-icon3.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="flexX line-h">
					<view class="flex-shrink p-r mr-3 overflow-h" v-for="(item,index) of mainData.coach" :key="index"
					 :data-id="item.id" 
					@click="Router.navigateTo({route:{path:'/pages/sijiao-detail/sijiao-detail?id='+$event.currentTarget.dataset.id}})">
						<image :src="item.bannerImg&&item.bannerImg[0]?item.bannerImg[0].url:''" class="storeImg2"></image>
						<view class="font-w py-2">{{item.name?item.name:''}}</view>
						<view class="flex1">
							<view class="font-22 color6">{{item.expertise?item.expertise:''}}</view>
							<view class="font-20 color9">累计 {{item.class?item.class:0}}节</view>
						</view>
						<view class="tjSgin">店长推荐</view>
					</view>
				</view>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="font-w t-indent20 line-h pt-4 pb-3 tit">门店课程</view>
				<view class="font-26 text-center flex1 p-2 bB-f5 time">
					<view class="wh80" v-for="(item,index) in timeList" :key="index" :class="timeCurr==index?'on':''" @click="changeTime(index)">
						<view class="pt-1">{{week[item.ds]}}</view>
						<view class="font-20 color6">{{item.m}}.{{item.d}}</view>
					</view>
				</view>
				<view class="shadow radius20 m-a p-r mb-3 overflow-h tkBox" 
				v-for="(item,index) in mainData.product" :key="index"
				@click="goToDetail(item)">
					<image :src="item.mainImg[0].url" class="kcImg"></image>
					<view class="px-2 py-3">
						<view class="font-30 flex1">
							<view class="font-w">{{item.title}}</view>
							<view><text class="price font-w">{{item.price}}</text>/{{item.score}}课时</view>
						</view>
						<view class="font-24 py-2">Alsue | {{item.start_time}}~{{item.end_time}}</view>
						<view class="flex">
							<view class="tag" v-for="(c_item,c_index) of item.description" :key="c_index">{{c_item}}</view>
						</view>
					</view>
					<view class="font-20 colorf kcSgin">差{{item.standard}}个人开课</view>
				</view>
			</view>
			
		</view>
		
		<!-- 客服 -->
		<image src="../../static/images/stores-details-icon1.png" class="kfImg" @click="isShow(1)"></image>
		<!-- 客服弹窗 -->
		<view class="bg-mask" v-show="kf_show">
			<view class="p-r kf">
				<view class="bg-white w-600 radius20 pb-5 font-24 p-r kfCon">
					<image src="../../static/images/stores-details-img4.png" class="wh120 kfLogo"></image>
					<view class="font-30 text-center py-2">店长：大白</view>
					<view class="mx-3 mb-2 px-3 py-2 flex kfBg radius10">
						<view>联系方式：</view>
						<image src="../../static/images/stores-details-icon18.png" class="wh50 mx-3"></image>
						<image src="../../static/images/stores-details-icon19.png" class="wh50 mx-3"></image>
					</view>
					<view class="mx-3 mb-5 px-3 py-2 kfBg radius10 line-h-lg">
						<view>联系方式：<br/>欢迎来到怪力牛运动{{mainData.name}}，本店24小时营业，疫情期间请做好个人防护，如有问题请联系微店长  {{mainData.phone}}（微信同号）</view>
					</view>
				</view>
				<image src="../../static/images/stores-details-icon20.png" class="wh30 m-4 xx" @click="isShow(1)"></image>
			</view>
		</view>
		
		<!-- 门店详情全部 -->
		<view class="bg-mask" v-show="sb_show">
			<view class="bg-white radius30-T p-a bottom-0 storeAll">
				<view class="p-r flexY h-100">
					<view class="font-34 pt-4 text-center">服务设备</view>
					
					<view class="py-3 font-30 font-w px-4">器械设备</view>
					<view class="px-5 flex flex-wrap">
						<block v-for="(item,index) in equipment" :key="index">
							<view v-if="item.key&&mainData[item.key]==1" class="flex4 font-24 color6 mb-3 bb" >
								<image :src="item.url" class="wh100 mb-2"></image>
								<view>{{item.name}}</view>
							</view>
						</block>
						
					</view>
					<view class="py-3 font-30 font-w px-4">基础服务</view>
					<view class="px-5 flex flex-wrap">
						<block v-for="(item,index) in service" :key="index">
							<view class="flex4 font-24 color6 mb-3 bb" v-if="item.key&&mainData[item.key]==1">
								<image :src="item.url" class="wh100 mb-2"></image>
								<view>{{item.name}}</view>
							</view>
						</block>
					</view>
				</view>
				<image src="../../static/images/stores-details-icon21.png" class="wh30 p-a m-2 right-0 z-index100 top-0" @click="isShow(2)"></image>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				timeCurr:0,
				kf_show:false,
				sb_show:false,
				equipment:[
					{url:'../../static/images/stores-details-icon11.png',name:'固定器械',key:'apparatus'},
					{url:'../../static/images/stores-details-icon12.png',name:'哑铃',key:'dumbbell'},
					{url:'../../static/images/stores-details-icon13.png',name:'深蹲架',key:'squat'},
					{url:'../../static/images/stores-details-icon14.png',name:'卧推',key:'bench'},
					{url:'../../static/images/stores-details-icon15.png',name:'动感单车',key:'bicycle'},
					{url:'../../static/images/stores-details-icon16.png',name:'跑步机',key:'run'},
					{url:'../../static/images/stores-details-icon17.png',name:'瑜伽垫',key:'yoga'}
				],
				service:[
					{url:'../../static/images/stores-details-icon2.png',name:'无线网络',key:'wifi'},
					{url:'../../static/images/stores-details-icon3.png',name:'储物柜',key:'cabinet'},
					{url:'../../static/images/stores-details-icon4.png',name:'更衣室',key:'locker'},
					{url:'../../static/images/stores-details-icon5.png',name:'饮水机',key:'water'},
					{url:'../../static/images/stores-details-icon6.png',name:'人脸识别',key:'face'},
					{url:'../../static/images/stores-details-icon9.png',name:'充电宝',key:'charge'},
					{url:'../../static/images/stores-details-icon10.png',name:'沐浴房',key:'bathroom'}
				],
				mainData:{},
				timeList:[],
				week:['周日','周一','周二','周三','周四','周五','周六'],
				searchItem: {
					thirdapp_id:2
				},
				chooseTimestap:0,
				start_time:0,
				distance:'',
				shopData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.timeList = self.$Utils.getFutureDateList(5);
			self.shopData = uni.getStorageSync('shopData')
			self.distance = uni.getStorageSync('shopData').distance;
			console.log('self.timeList',self.timeList);
			self.chooseTimestap = self.timeList[0]['stime'];
			self.start_time = ['between',
				[self.chooseTimestap,self.chooseTimestap+24*3600000],
			];
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		methods: {
			
			goToDetail(item){
				const self = this;
				uni.setStorageSync('leagueClassDetail',item);
				self.Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=0'}});
			},
			
			changeTime(i){
				const self = this;
				self.timeCurr = i;
				self.chooseTimestap = self.timeList[i]['stime'];
				self.start_time = ['between',[self.chooseTimestap,self.chooseTimestap+86400000]];
				self.getMainData();
			},
			
			
			getMainData() {
				var self = this;
				const postData = {};
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.id = self.id;
				postData.getAfter = {
					active:{
						tableName:'Article',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							menu_id:2
						},
						condition:'='
					},
					coach:{
						tableName:'Coach',
						middleKey:'user_no',
						key:'shop_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					product:{
						tableName:'Product',
						middleKey:'user_no',
						key:'shop_no',
						searchItem:{
							status:1,
							type:1,
							start_time:self.start_time
						},
						condition:'='	
					}
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.mainData = res.info.data[0];
						for (var i = 0; i < self.mainData.product.length; i++) {
							self.mainData.product[i].description = self.mainData.product[i].description.split(',');
							self.mainData.product[i].start_time = self.$Utils.timeto(parseInt(self.mainData.product[i].start_time),'ymd-hms')
							self.mainData.product[i].end_time = self.$Utils.timeto(parseInt(self.mainData.product[i].end_time),'ymd-hms')
						}
					}
					console.log('mainData',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.shopGet(postData, callback);
			},
			
			isShow(type){
				const self = this;
				if(type==1){
					self.kf_show = !self.kf_show;
				}else if(type == 2){
					self.sb_show = !self.sb_show;
				}
			}
			
		}
	}
</script>

<style scoped>
.banner{width: 100%;height: 510rpx;}
.content{margin-top: -50rpx;}
.sq{top: -10rpx;right: -10rpx;}
.dz{width: 514rpx;}
.content .tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #FF633A;position: absolute;left: 20rpx;}
.storeImg1{width: 100%;height: 200rpx;border-radius: 10rpx;}
.storeImg2{width: 370rpx;height: 200rpx;border-radius: 10rpx;}
.time .on{background-color: #FF633A;border-radius: 50%; color: #fff!important;}
.time .on view{color: #fff;}

.kfImg{width: 130rpx;height: 110rpx;position: fixed;top: 60%;right: 0;}
.kfLogo{position: absolute;left: 0;right: 0;margin: auto;top: -50rpx;}
.kf{margin: 40% 75rpx 0;}
.kfCon{padding-top: 70rpx;}
.kfBg{background-color: #FFF9F5;}
.xx{margin: 100rpx auto;}

.storeAll{height: 90%;width: 100%;}
.bb{margin-right: 85rpx;}
.bb:nth-child(4n){margin-right: 0;}
</style>
