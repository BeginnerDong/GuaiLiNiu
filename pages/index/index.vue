<template>
	<view>
		
		<!-- banner -->
		<container>
			<ls-swiper :list="sliderData" imgKey="imgUrl" :loop="true" :dots='true' :autoplay='true' height='125' imgRadius='10' imgWidth='355'/>
		</container>
		
		<!-- 金刚区 -->
		<view class="flex2 py-3">
			<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/leagueClass/leagueClass?shop_no='+shopData.user_no}})">
				<image src="../../static/images/home-icon.png" class="home-icon"></image>
				<view>团课</view>
			</view>
			<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/payLeagueClass/payLeagueClass?shop_no='+shopData.user_no}})">
				<image src="../../static/images/home-icon1.png" class="home-icon"></image>
				<view>付费团课</view>
			</view>
			<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/sijiao/sijiao?shop_no='+shopData.user_no}})">
				<image src="../../static/images/home-icon2.png" class="home-icon"></image>
				<view>私教</view>
			</view>
		</view>
		
		<!-- 门店列表 -->
		<view class="px-2 p-r">
			<image src="../../static/images/home-img.png" class="homeBg" :style="{marginBottom:is_show?'280rpx':'130rpx'}"></image>
			
			<view class="p-r line-h px-3">
				<view class="d-flex a-start j-sb">
					<view class="font-30 font-w py-3">门店列表</view>
					<view class="font-22 p-1 b-e1 radius" @click="Router.navigateTo({route:{path:'/pages/store/store'}})">逛逛其他店</view>
				</view>
				<view @click="Router.navigateTo({route:{path:'/pages/storeDetail/storeDetail?id='+shopData.id}})">
					<view class="flex py-3">
						<view class="font-34">{{shopData.name?shopData.name:''}}</view>
						<view class="font-20 colorf p-r sq">
							<image src="../../static/images/home-icon5.png" class="sq-icon"></image>
							<view class="top-0 p-a t-indent10 line-h-md">社区店</view>
						</view>
					</view>
					<view class="flex1">
						<image src="../../static/images/home-icon3.png" class="homeDz-icon"></image>
						<view class="flex-1 flex">
							<view class="font-24 pr-2">{{shopData.address?shopData.address:''}}</view>
							<view class="font-20 bg-f5 d-inline-block line-h-md px-1">{{shopData.distance}}KM</view>
						</view>
						<image src="../../static/images/home-icon4.png" class="R-icon"></image>
					</view>
				</view>
				
				<view class="pt-3" v-if="activeData.length>0">
					<view class="font-30 font-w py-3">门店活动</view>
					<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff" class="banner1">
						<swiper-item v-for="(item,index) in activeData" :key="index">
							<view   :data-id="item.id" 
									class="swiper-item" 
									@click="Router.navigateTo({route:{path:'/pages/limitedTime/limitedTime?id='+$event.currentTarget.dataset.id}})"
							>
								<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="radius10"></image>
							</view>
						</swiper-item>
					</swiper>
				</view>
				
				<view class="pt-2">
					<view class="font-30 py-3 flex1">
						<view class="font-w">门店课程</view>
						<view class="flex" @click="Router.navigateTo({route:{path:'/pages/sijiao-course/sijiao-course?shop_no='+shopData.user_no}})">
							<view class="font-26 color9 pr-2">全部</view>
							<image src="../../static/images/the-order-icon3.png" class="R-icon"></image>
						</view>
					</view>
					<view class="line-h flexX py-1">
						<view class="shadowM radius10 flex-shrink overflow-h mr-3 p-r"  v-for="(item,index) in classData" :key="index" @click="goToDetail(item,1)">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="kcImg"></image>
							<view class="p-3">
								<view class="font-30 font-w">
									{{item.title?item.title:''}}
									<text class="color9 font-24 pl-1">({{item.course_type==1?'免费团课':(item.course_type==2?'付费团课':'私教课')}})</text>
								</view>
								<view class="font-24 pt-3">{{item.coach&&item.coach[0]?item.coach[0].name:''}} | 有效期：{{item.duration}}天</view><!-- {{item.start_time}}~{{item.end_time}} -->
							</view>
							<view class="font-20 colorf kcSgin">差{{item.is_book?item.standard-item.is_book:item.standard}}个人开课</view>
						</view>
					</view>
				</view>
				
				<view class="pt-2 pb-3 p-r sijiaoBox">
					<view class="font-30 py-3 flex1">
						<view class="font-w">推荐私教</view>
						<view class="flex" @click="Router.navigateTo({route:{path:'/pages/store-sijiao/store-sijiao?shop_no='+shopData.user_no}})">
							<view class="font-26 color9 pr-2">全部</view>
							<image src="../../static/images/the-order-icon3.png" class="R-icon"></image>
						</view>
					</view>
					<view class="line-h">
						<block  v-for="(item,index) in coachData" :key="index">
							<view class="shadowM radius10 flex1 p-2 mb-3" v-show="index<2" @click="goToDetail(item,2)">
								<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="sjImg"></image>
								<view class="p-3 flex-1">
									<view class="flex">
										<view class="font-30 font-w pr-2">{{item.name}}</view>
										<view class="flex">
											<image src="../../static/images/home-icon6.png" class="zs-icon"></image>
											<view class="colorZS font-24 pl-1">专业证书（{{item.certificate.length}}）</view>
										</view>
									</view>
									<view class="flex py-4">
										<view class="tag" v-for="(c_item,c_index) of item.expertise" :key="c_index">{{c_item}}</view>
									</view>
									<view class="flex">
										<view class="price font-26 pr-5">{{item.class&&item.class[0]?item.class[0].price:'0'}}/节</view>
										<view class="font-22 color9 pl-4">累计：{{mainData.class?mainData.class:0}}节</view>
									</view>
								</view>
							</view>
						</block>
						
					</view>
					<view class="criBtn p-aX bottom-0"
					@click="Router.navigateTo({route:{path:'/pages/storeDetail/storeDetail?id='+shopData.id}})">进入店铺</view>
				</view>
				
			</view>
		</view>
		
		
		<view v-if="id_free==0" class="homeBto mx-2 my-5 radius10 p-3 flex1 p-r" style="position: fixed;bottom: 80rpx;" v-show="is_show">
			<view class="flex1"
			 @click="Router.navigateTo({route:{path:'/pages/experienceCoupon/experienceCoupon'}})">
				<image src="../../static/images/home-img4.png" class="Img80"></image>
				<view class="pl-2 pr-5 flex-1">店长送你一份信任见面礼，如需到店使用请提前预约~</view>
				<view class="criBtn criBtn1">去领取</view>
			</view>
			<image src="../../static/images/my-class-icon1.png" class="x-icon1" @click="isShow"></image>
		</view>
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import container from '../../components/container/index.vue'
	import LsSwiper from '../../components/ls-swiper/index.vue'
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: true,
				sliderData:[],
				shopData:{},
				activeData:[],
				coachData:[],
				classData:[],
				id_free:1
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getSliderData'], self);
			if(!uni.getStorageSync('shopData')){
				self.getLocation();
			}
		},
		
		onShow() {
			const self = this;
			if(self.shopData.name != uni.getStorageSync('shopData').name){
				self.shopData = uni.getStorageSync('shopData')
				self.getActiveData();
				self.getCoachData();
				self.getClassData();
				self.id_free = uni.getStorageSync('user_info').info.id_free;
			}
		},
		
		components: {
			container,
			LsSwiper
		},
		
		methods: {
			
			goToDetail(item,type){
				const self = this;
				if(type == 1){
					if(typeof(item.description) == 'string'){
						item.description = item.description.split(',')
					}
					uni.setStorageSync('leagueClassDetail',item);
					if(item.course_type == 1){
						self.Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=0'}});
					}else if(item.course_type == 2){
						self.Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=1'}});
					}else if(item.course_type == 3){
						item.coachName = item.coach[0].name
						uni.setStorageSync('sijiaoCourseDetail',item);
						self.Router.navigateTo({route:{path:'/pages/sijiao-classDetail/sijiao-classDetail'}});
					}
				}else{
					self.Router.navigateTo({route:{path:'/pages/sijiao-detail/sijiao-detail?coach_no='+item.user_no}})
				}
			},
			
			getLocation(){
				const self = this;
				uni.getLocation({
					type:'wgs84',
					success(res) {
						self.getShopData(res.longitude,res.latitude)
					}
				})
			},
			
			getShopData(longitude,latitude) {
				var self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2,
					user_type:1
				};
				postData.order = {
					distance:'asc',
					longitude:longitude,
					latitude:latitude
				};
				var callback = function(res){
					if(res.info.data.length>0){
						for(var i=0;i<res.info.data.length;i++){
							res.info.data[i].distance = parseFloat(res.info.data[i].distance).toFixed(2);
						}
						console.log('店铺',res.info.data)
						self.shopData = res.info.data[0];
						uni.setStorageSync('shopData', res.info.data[0]);
						uni.setStorageSync('shopList', res.info.data);
						uni.setStorageSync('shopListTime', (new Date()).getTime()+30000);
						self.getActiveData();
						self.getCoachData();
						self.getClassData();
						self.id_free = uni.getStorageSync('user_info').info.id_free;
					}else{
						uni.showModal({
							title:'提示',
							content:'更多门店筹备中...',
							showCancel:false
						})
					};
					self.$Utils.finishFunc('getLocation');
				};
				self.$apis.shopGet(postData, callback);
			},
			
			getClassData() {
				var self = this;
				const postData = {};
				postData.paginate = {
					count: 0,
					currentPage:1,
					pagesize:5,
					is_page:true,
				};
				postData.searchItem = {
					thirdapp_id:2,
					type:1,
					shop_no:uni.getStorageSync('shopData').user_no
				};
				postData.getAfter = {
					coach:{
						tableName:'Coach',
						middleKey:'coach_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.classData = res.info.data;
						for (var i = 0; i < self.classData.length; i++) {
							self.classData[i].start_time = self.$Utils.timeto(parseInt(self.classData[i].start_time),'hm')
							self.classData[i].end_time = self.$Utils.timeto(parseInt(self.classData[i].end_time),'hm')
						}
					}
					// console.log("class",self.classData)
				};
				self.$apis.productGet(postData, callback);
			},
			
			getActiveData() {
				var self = this;
				const postData = {};
				postData.paginate = {
					count: 0,
					currentPage:1,
					pagesize:5,
					is_page:true,
				};
				postData.searchItem = {
					thirdapp_id:2,
					menu_id:2,
					user_no:uni.getStorageSync('shopData').user_no
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.activeData = res.info.data
					}
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getCoachData() {
				var self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				if(self.shopData){
					postData.searchItem.shop_no = self.shopData.user_no
				}else{
					postData.searchItem.shop_no = uni.getStorageSync('shopData').user_no
				}
				postData.getAfter = {
					class:{
						tableName:'Product',
						middleKey:'user_no',
						key:'coach_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					course:{
						tableName:'Course',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							is_book:1
						},
						condition:'=',
						compute:{
							num:[
								'count',
								'count',
								{status:1,is_book:1}
							]
						}
					}
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.coachData = res.info.data;
						for (var i = 0; i < self.coachData.length; i++) {
							self.coachData[i].expertise = self.coachData[i].expertise.split(',')
						}
					}
					
				};
				self.$apis.coachGet(postData, callback);
			},
			
			getSliderData() {
				var self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					menu_id:1
				};
				var callback = function(res){
					if(res.info.data.length>0){
						for(var i=0; i<res.info.data.length;i++){
							self.sliderData.push(res.info.data[i].mainImg[0].url)
						}
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			// getMainData() {
			// 	const self = this;
				
			// },
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			}
			
		}
	};
</script>
<style scoped>
swiper{height: 250rpx;}
.banner{width: 710rpx;height: 250rpx;border-radius:20rpx;overflow: hidden;}
.banner image{width: 100%;height: 250rpx;border-radius:20rpx;}
.home-icon{width: 112rpx;height: 112rpx;margin-bottom: 20rpx;}
.homeBg{width: 722rpx;height: 1818rpx;position: absolute;top: 0;left: 0;right: 0;margin: 0 auto 280rpx;}
.sq{top: -10rpx;right: -10rpx;}
.banner1{width: 650rpx;height: 200rpx;}
.banner1 image{width: 650rpx;height: 200rpx;}
.kcImg{width: 420rpx;height: 230rpx;}

.sjImg{width: 160rpx;height: 196rpx;border-radius: 10rpx;}
.homeBto{background-color: #3F2E2A;font-size: 24rpx;color: #fff;}
.criBtn1{width: 110rpx;line-height: 40rpx;border-radius: 20rpx;}

.sijiaoBox{min-height: 800rpx;}

/* .x-icon1{width: 40rpx;height: 40rpx;} */

</style>
