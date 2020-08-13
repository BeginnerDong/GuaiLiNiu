<template>
	<view>
		
		<view class="p-r">
			<image src="../../static/images/sijiao details-img.png" class="sjBg"></image>
			<view class="flex1 p-2 mb-3 line-h p-aXY">
				<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="sjImg"></image>
				<view class="p-3 flex-1">
					<view class="flex">
						<view class="font-30 font-w pr-2">{{mainData.name?mainData.name:''}}</view>
						<view class="flex" @click="isShow">
							<image src="../../static/images/home-icon6.png" class="zs-icon"></image>
							<view class="colorZS font-24 pl-1">专业证书（{{mainData.certificate?mainData.certificate.length:''}}）</view>
						</view>
					</view>
					<view class="py-4">{{mainData.phone?mainData.phone:''}}</view>
					<view>好评率{{mainData.good?mainData.good:''}} | 累计上{{mainData.class?mainData.class:0}}节课</view>
				</view>
			</view>
		</view>
		
		<view class="pb-4 px-2">
			<view class="pb-3 font-w t-indent20 font-30 line-h pt-4 tit">教练相册</view>
			<view class="flexX">
				<image v-for="(item,index) in mainData.albumImg" :key="index" :src="item.url" class="sjImg1"></image>
			</view>
		</view>
		
		<view class="pb-4 px-2">
			<view class="pb-3 font-w t-indent20 font-30 line-h tit">自我介绍</view>
			<view class="shadowM p-3 radius10 font-26">
				{{mainData.introduce?mainData.introduce:''}}
			</view>
		</view>
		
		<view class="pb-4 px-2">
			<view class="pb-3 font-w t-indent20 font-30 line-h tit">擅长领域</view>
			<view class="flexX">
				<view class="tag" v-for="(item,index) of mainData.expertise" :key="index">{{item}}</view>
			</view>
		</view>
		
		<view class="pb-4 px-2">
			<view class="pb-3 font-w t-indent20 font-30 line-h tit">服务门店</view>
			<view class="colorM borderM d-inline-block font-26 px-1 radius">{{mainData.shop?mainData.shop.name:''}}</view>
		</view>
		
		<view class="pb-1 px-2">
			<view class="flex1">
				<view class="font-w t-indent20 font-30 line-h tit">所授课程</view>
				<view class="color6 flex1" @click="Router.navigateTo({route:{path:'/pages/sijiao-courses/sijiao-courses?coach_no='+mainData.user_no}})">
					<view>查看全部</view>
					<image src="../../static/images/the-order-icon3.png" class="R-icon ml-1"></image>
				</view>
			</view>
			<view class="flexX py-3">
				<block v-for="(item,index) in courseData" :key="index">
					<view class="shadowM radius10 overflow-h flex-shrink mr-2" @click="goToDetail(item)">
						<image :src="item.mainImg[0].url" class="kcImg"></image>
						<view class="py-3 px-2 line-h">
							<view class="font-30 font-w">{{item.title}}</view>
							<view class="pt-3"><text class="font-30 font-w price">{{item.price}}</text>/{{item.score}}课时</view>
						</view>
					</view>
				</block>
			</view>
		</view>
		
		<view class="pb-4 px-2">
			<view class="font-w t-indent20 font-30 line-h tit">学员评价</view>
			
			<block v-for="(item,index) in remarkData" :key="index">
				<view class="bB-f5 py-3">
					<view class="font-24 flex1">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="wh70 radius-5"></image>
						<view class="color6 flex-1 px-2">{{item.title}}</view>
						<view class="color9">{{item.create_time}}</view>
					</view>
					<view class="font-26 pt-3">
						{{item.description}}
					</view>
					<view class="flex flex-wrap">
						<image v-for="(c_item,c_index) in item.bannerImg" :key="c_index" :src="c_item.url" class="wh180 mt-2 mr-2"></image>
					</view>
				</view>
			</block>
		</view>
		
		
		<view class="bg-mask" v-show="is_show">
			<view class="banner">
				<swiper :indicator-dots="false" :autoplay="false" :interval="3000" :duration="1000" >
					<block v-for="(item,index) in mainData.certificate" :key="index">
						<swiper-item>
								<image :src="item.url" ></image>
						</swiper-item>
					</block>
				</swiper>
				
				<view class="xx wh50 p-r" @click="isShow">
					<image src="../../static/images/stores details-icon20.png" class="wh20 p-aXY m-a"></image>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				mainData:{},
				remarkData:[],
				isLoadAll:false,
				courseData:[],
				id:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.coach_no = options.coach_no;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			
			self.$Utils.loadAll(['getMainData'], self);
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMessageData()
			};
		},
		
		methods: {
			
			goToDetail(item){
				const self = this;
				item.coachName = {};
				item.coachName = self.mainData.name;
				// item.description = item.description.split(',')
				uni.setStorageSync('sijiaoCourseDetail',item);
				self.Router.navigateTo({route:{path:'/pages/sijiao-classDetail/sijiao-classDetail'}});
			},
			
			getMainData() {
				var self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					user_no:self.coach_no,
					id:self.id
				};
				postData.getAfter = {
					shop:{
						tableName:'Shop',
						middleKey:'shop_no',
						key:'user_no',
						searchItem:{
							status:1,
						},
						condition:'=',
						info:['name']
					},
					product:{
						tableName:'Product',
						middleKey:'user_no',
						key:'coach_no',
						searchItem:{
							status:1,
						},
						condition:'=',
						
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
						self.mainData = res.info.data[0];
						self.mainData.expertise = self.mainData.expertise.split(',')
						for(var i=0;i<self.mainData.product.length;i++){
							if(self.mainData.product[i].course_type == 3){
								self.courseData.push(self.mainData.product[i]);
								self.courseData.coach = {};
								self.courseData.coach.name = self.mainData.name;
							}
						}
					};
					self.getMessageData();
					console.log('course',self.courseData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.coachGet(postData, callback);
			},
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			},
			
			getMessageData(isNew){
				const self = this;
					if (isNew) {
						self.remarkData = [];
						self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
					};
					const postData = {};
					//postData.tokenFuncName = 'getProjectToken';
					postData.paginate = self.$Utils.cloneForm(self.paginate);
					postData.searchItem = {
						thirdapp_id:2,
						passage2:self.mainData.user_no,
					};
					const callback = (res) => {
						if (res.info.data.length > 0) {
							self.remarkData.push.apply(self.remarkData, res.info.data);
						};
						uni.setStorageSync('canClick', true);
						self.$Utils.finishFunc('getMessageData');
					};
					self.$apis.messageGet(postData, callback);
				
			}
		}
	}
</script>

<style scoped>
.sjBg{width: 100%;height: 270rpx;}
.sjImg{width: 180rpx;height: 220rpx;border-radius: 10rpx;}
.tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #FF633A;position: absolute;left: 20rpx;}
.sjImg1{width: 240rpx;height: 160rpx;border-radius: 10rpx;margin-right: 20rpx;flex-shrink: 0;}
.tag{margin-right: 20rpx;}
.kcImg{width: 400rpx;height: 220rpx;}

.banner{width: 750rpx;height: 400rpx;margin-top: 40%;}
swiper{width: 100%;height: 400rpx;margin: 0 auto;}
swiper image{width: 600rpx;height: 400rpx;}
swiper-item{overflow: inherit;width: 600rpx;margin: 0 75rpx;}

.xx{border: 2px solid #fff;border-radius: 50%;display: inline-block;margin-left: -25rpx;left: 50%;margin-top: 20%;}
</style>
