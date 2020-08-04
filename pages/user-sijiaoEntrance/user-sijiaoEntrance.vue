<template>
	<view>
		
		<view class="p-r">
			<view class="backBox" @click="Router.back({route:{dalta:-1}})">
				<image src="../../static/images/back-icon1.png" class="back"></image>
			</view>
			
			<image src="../../static/images/about-img.png" class="userBg"></image>
			<view class="p-aXY">
				<view>
					<view class="head" :style="{marginTop:statusBar+'px'}"></view>
					<view class="colorf mx-2 my-3 flex p-r">
						<image src="../../static/images/about-img1.png" class="wh110"></image>
						<view class="font-32 pl-2">哆啦A梦</view>
						<view class="p-a top-0 right-0" @click="checkOut">退出</view>
					</view>
				</view>
				
				<view class="bg-white font-24 px-2 py-5 flex1">
					<view class="flex4 shadowM radius10 box" @click="Router.navigateTo({route:{path:'/pages/user-appointment/user-appointment'}})">
						<image src="../../static/images/about-icon.png" class="icon1"></image>
						<view>团课</view>
					</view>
					<view class="flex4 shadowM radius10 box" @click="Router.navigateTo({route:{path:'/pages/user-VIP/user-VIP'}})">
						<image src="../../static/images/about-icon1.png" class="icon2"></image>
						<view>私教</view>
					</view>
				</view>
				
			</view>
		</view>
		
		
				
		
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				Router:this.$Router,
				statusBar: app.globalData.statusBar
			}
		},
		onLoad(){
			const self  = this;
			
			self.$Utils.loadAll(['getUserData'], self);
		},
		methods: {
			
			checkOut(){
				const self = this;
				uni.removeStorageSync('coach_token');
				uni.removeStorageSync('login');
				self.getUserData();
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getCoachToken';
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
		}
	}
</script>

<style scoped>
.head{height: 100rpx;}
.userBg{height: 524rpx;}
.icon1{width: 70rpx;height: 58rpx;margin-bottom: 30rpx;}
.icon2{width: 60rpx;height: 61rpx;margin-bottom: 30rpx;}
.box{width: 340rpx;height: 160rpx;}
</style>
