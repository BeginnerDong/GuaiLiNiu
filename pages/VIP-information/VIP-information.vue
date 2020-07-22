<template>
	<view class="px-2 font-30">
		
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members information-icon.png" class="inforIcon"></image>
			<view class="flex-1 pl-2">姓名</view>
			<input type="text" value="" placeholder="请输入" />
		</view>
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members information-icon1.png" class="inforIcon1"></image>
			<view class="flex-1 pl-2">年龄</view>
			<input type="text" value="" placeholder="请输入" />
		</view>
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members information-icon2.png" class="inforIcon2"></image>
			<view class="flex-1 pl-2">性别</view>
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
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members information-icon3.png" class="inforIcon"></image>
			<view class="flex-1 pl-2">电话</view>
			<input type="text" value="" placeholder="请输入" />
		</view>
		
		<view class="btnAuto">确定</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.inforIcon{width: 26rpx;height: 30rpx;}
.inforIcon1{width: 32rpx;height: 30rpx;}
.inforIcon2{width: 32rpx;height: 28rpx;}
input{width: 400rpx;text-align: right;font-size: 26rpx;}

.btnAuto{margin-top: 200rpx;}
</style>
