<template>
	<view class="font-26 line-h">
		
		<view class="top text-center py-2 colorf">距离活动结束：<text class="font-32">02天12时11分15秒</text></view>
		<image src="../../static/images/Vpreferential-img.png" class="limitImg"></image>
		<veiw class="d-flex a-start px-2 mb-5">
			<image src="../../static/images/preferential-icon.png" class="wh30 mr-2"></image>
			<view>
				<view class="font-30 font-w pb-3">西安外事学院</view>
				<view>活动介绍：原价年卡999直降200，现在只需799</view>
			</view>
		</veiw>
		
		<view class="bg-white radius10 mx-2 py-5 mb-3">
			<view class="flex j-center">
				<image src="../../static/images/preferential-icon1.png" class="gk-icon"></image>
				<view class="font-36 px-2">购卡优惠</view>
				<image src="../../static/images/preferential-icon2.png" class="gk-icon"></image>
			</view>
			<view class="font-24 color6 text-center py-3">会员卡仅限西安外事学院店使用</view>
			<view class="flex1 px-3">
				<image src="../../static/images/preferential-img1.png" class="nkImg"></image>
				<view class="flex-1 pl-2">
					<view class="font-24 pb-4">年卡直降200</view>
					<view class="colorR">799元<text class="price line-through font-22 pl-3">999</text></view>
				</view> 
				<view class="criBtn" @click="Router.navigateTo({route:{path:'/pages/limitedTime-may/limitedTime-may'}})">立即购买</view>
			</view>
		</view>
		
		<view class="bg-white radius10 mx-2 py-5 mb-5">
			<view class="flex j-center">
				<image src="../../static/images/preferential-icon1.png" class="gk-icon"></image>
				<view class="font-36 px-2">活动规则</view>
				<image src="../../static/images/preferential-icon2.png" class="gk-icon"></image>
			</view>
			<view class="pt-3 px-3 color6">
				1.本活动所有用户皆可参加
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>
<style>
page{background-color: #63D1F8;}
</style>
<style scoped>
.top{background-color: #1C3741;}
.limitImg{width: 639rpx;height: 437rpx;margin: 50rpx auto 20rpx;}
.gk-icon{width: 23rpx;height: 16rpx;}
.nkImg{width: 150rpx;height: 110rpx;}
.criBtn{width: 160rpx;}
.price{color: #666;}
</style>
