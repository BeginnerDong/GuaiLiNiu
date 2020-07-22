<template>
	<view class="font-30">
		
		<view class="px-2 bg-white">
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">上传照片</view>
				<image src="../../static/images/img3.png" class="scImg"></image>
			</view>
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">姓名</view>
				<input type="text" value="" placeholder="请输入" />
			</view>
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">年龄</view>
				<input type="text" value="" placeholder="请输入" />
			</view>
			<view class="flex1 py-4">
				<view class="flex-1">性别</view>
				<view class="font-26 flex">
					<view class="flex">
						<image src="../../static/images/members information-icon4.png" class="wh32 mr-1"></image>
						<!-- <image src="../../static/images/members information-icon5.png" class="wh32 mr-1"></image> -->
						<view>男</view>
					</view>
					<view class="flex pl-5">
						<!-- <image src="../../static/images/members information-icon4.png" class="wh32 mr-1"></image> -->
						<image src="../../static/images/members information-icon5.png" class="wh32 mr-1"></image>
						<view>女</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="px-2">
			<view class="btnAuto" @click="Router.navigateTo({route:{path:'/pages/experienceUser/experienceUser'}})">确定</view>
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
page{background-color: #f5f5f5;}
</style>
<style scoped>
.inforIcon{width: 26rpx;height: 30rpx;}
.inforIcon1{width: 32rpx;height: 30rpx;}
.inforIcon2{width: 32rpx;height: 28rpx;}
input{width: 400rpx;text-align: right;font-size: 26rpx;}
.scImg{width: 210rpx;height: 130rpx;}

.btnAuto{margin-top: 200rpx;}
</style>