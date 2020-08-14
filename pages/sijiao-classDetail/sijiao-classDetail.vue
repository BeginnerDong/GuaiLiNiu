<template>
	<view>
		
		<!-- banner -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff" class="banner">
			<swiper-item v-for="(item,index) in mainData.bannerImg" :key="index">
				<image :src="item.url" ></image>
			</swiper-item>
		</swiper>
		
		<view class="content bg-white radius30-T px-2 p-r content">
			<view class="pb-4 bB-f5">
				<view class="flex pt-5 font-40 font-w">{{mainData.title}}</view>
				<button open-type="share" class="shareSgin"  :data-id="mainData.id">
					<image src="../../static/images/course-icon2.png" class="fx-icon"></image>
					<view>分享</view>
				</button>
				<view class="flex pt-3">
					<view class="bR-e1 pr-2 flex">
						<image src="../../static/images/pay-for-courses-icon.png" class="wh30 mr-1"></image>
						私教课
					</view>
					<view class="flex pl-2">
						<view class="tag" v-for="(item,index) in mainData.description" :key="index">{{item}}</view>
					</view>
				</view>
				
				<view class="font-20 pt-3"><text class="font-36 font-w price">{{mainData.price}}</text>/{{mainData.score}}课时</view>
			</view>
			
			<view class="font-26 pt-4 ">
				<view class="d-flex a-start pb-4">
					<image src="../../static/images/pay-for-courses-icon1.png" class="rq-icon mt"></image>
					<view class="flex-1 pl-2">适合人群：{{mainData.fit}}</view>
				</view>
				<view class="d-flex a-start pb-4">
					<image src="../../static/images/members-icon6.png" class="fw-icon mt"></image>
					<view class="flex-1 pl-2">服务教练：{{mainData.coachName}}</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="p-3 radius10 shadowM p-r">
				<view class="font-w t-indent20 font-30 line-h tit">课程内容</view>
				<view class="font-26 color9 pt-3">
					<view v-html="mainData.content"></view>
				</view>
				<view class="p-a flex1 left-0 right-0 mt-2 px-2">
					<image src="../../static/images/course-icon.png" class="courIcon"></image>
					<image src="../../static/images/course-icon1.png" class="courIcon"></image>
				</view>
			</view>
			<view class="p-3 radius10 shadowM p-r mt-2">
				<view class="font-w t-indent20 font-30 line-h tit">课程规则</view>
				<view class="font-26 color9 pt-3">
					<view v-html="mainData.rule"></view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="font-w t-indent20 font-30 line-h tit">约课流程</view>
			<view class="font-24 flex1 pt-4">
				<view class="flex1 flex-1 pr-5">
					<view class="LC flex4">
						<image src="../../static/images/pay-for-courses-icon5.png"></image>
						<view>1.购买</view>
					</view>
					<view class="LC flex4">
						<image src="../../static/images/pay-for-courses-icon6.png"></image>
						<view>2.约课</view>
					</view>
					<view class="LC flex4">
						<image src="../../static/images/pay-for-courses-icon7.png"></image>
						<view>3.签到</view>
					</view>
					<view class="LC flex4">
						<image src="../../static/images/pay-for-courses-icon8.png"></image>
						<view>4.评价</view>
					</view>
				</view>
				<view class="flex4 colorM pl-5 p-r GZ"
				 @click="Router.navigateTo({route:{path:'/pages/ruleDetail/ruleDetail'}})">
					<image src="../../static/images/pay-for-courses-icon9.png" class="lcIcon5"></image>
					<view>规则详情</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="font-w t-indent20 font-30 line-h tit">用户评价</view>
			<block  v-for="(item,index) in remarkData" :key="index">
				<view class="bB-f5 py-3">
					<view class="font-24 flex1">
						<image src="../../static/images/pay-for-courses-img3.png" class="wh70"></image>
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
		<view class="f5Bj-H20"></view>
		
		
		<view style="height: 100rpx;"></view>
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 p-2 bT-e1">
			<view class="font-26">已预约{{mainData.is_book?mainData.is_book:0}}/{{mainData.max}}人，还差<text class="colorR">{{mainData.is_book?mainData.standard-mainData.is_book:mainData.standard}}</text>人开课</view>
			<view class="criBtn" @click="goOrder">立即购买</view>
		</view>
		
		
		<!-- 不是会员弹框 -->
		<view class="bg-mask" v-show="is_show">
			<view class="noVip bg-white radius10 text-center line-h m-a">
				<view class="py-2">
					<view class="pt-5 pb-4">很抱歉，您还不是怪力牛会员</view>
					<view class="pb-5 color9 font-24">成为会员，团课免费预约</view>
				</view>
				<view class="flex bT-f5">
					<view class="py-4 w-50 bR-f5" @click="isShow">我知道了</view>
					<view class="py-4 w-50 colorM">去看看</view>
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
				type:'Array',
				searchItem: {
					thirdapp_id: 2,
					type: 1,
					user_type: 1
				}
			}
		},
		onLoad(options){
			const self = this;
			if(options.id){
				self.searchItem.id = options.id;
				self.getMainData()
			}else{
				if(uni.getStorageSync('leagueClassDetail')){
					self.mainData = uni.getStorageSync('sijiaoCourseDetail');
				}else{
					uni.showToast({
						title:'课程信息不存在',
						duration:2000
					})
					self.$Router.navigateTo({route:{path:'/pages/index/index'}})
				}
			}
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			onShareAppMessage: function( options ){
			　　var that = this;
				
				var path = '/pages/sijiao-classDetail/sijiao-classDetail?id='+options.target.dataset.id;
				console.log('path',path);
			　　// 设置菜单中的转发按钮触发转发事件时的转发内容
			　　var shareObj = {
			　　　　title: '怪力牛运动——'+that.mainData.title,        // 默认是小程序的名称(可以写slogan等)
			　　　　path: path,        // 默认是当前页面，必须是以‘/’开头的完整路径
			　　　　imgUrl: '',     //自定义图片路径，可以是本地文件路径、代码包文件路径或者网络图片路径，支持PNG及JPG，不传入 imageUrl 则使用默认截图。显示图片长宽比是 5:4
			　　};
			　　return shareObj;
			},
			
			goOrder(){
				const self = this;
				self.mainData.shopInfor = self.shopData;
				uni.setStorageSync('sijiaoCourseDetail',self.mainData);
				self.$Router.navigateTo({route:{path:'/pages/leagueClass-order/leagueClass-order'}})
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
						relation_table:'product',
						relation_id:self.mainData.id,
					};
					const callback = (res) => {
						if (res.info.data.length > 0) {
							self.remarkData.push.apply(self.remarkData, res.info.data);
						};
						uni.setStorageSync('canClick', true);
						self.$Utils.finishFunc('getMessageData');
					};
					self.$apis.messageGet(postData, callback);
			},
			
			getMainData(){
				const self = this;
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data[0]);
						self.mainData.description = self.mainData.description.split(',');
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			
		}
	}
</script>

<style scoped>
.banner{width: 100%;height: 580rpx;}
.content{margin-top: -50rpx;}
.rq-icon{width: 32rpx;height: 28rpx;}
.fw-icon{width: 36rpx;height: 29rpx;}
.jlImg{width: 180rpx;height: 220rpx;border-radius: 10rpx;}
.tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #FF633A;position: absolute;left: 20rpx;}
.LC image{width: 52rpx;height: 48rpx;margin-bottom: 30rpx;}
.lcIcon5{width: 40rpx;height: 44rpx;margin-bottom: 30rpx;}
.GZ::before{content: "";border-left: 1px solid #e1e1e1;position: absolute;left: 0;height: 60rpx;}
.courIcon{width: 26rpx;height: 43rpx;}
.criBtn{line-height: 80rpx;border-radius: 40rpx;margin: 0;width: 260rpx;}

.noVip{width: 620rpx;margin-top: 65%;}
</style>
