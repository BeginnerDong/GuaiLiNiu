<template>
	<view>
		
		<image src="../../static/images/the loginl-img.png" class="login-logo"></image>
		
		<view class="flex m-75 bB-f5 pb-3 mb-5">
			<image src="../../static/images/the loginl-icon.png" class="zh-icon"></image>
			<input type="text" value="" placeholder="请输入账号" />
		</view>
		<view class="flex m-75 bB-f5 pb-3 mb-5">
			<image src="../../static/images/the loginl-icon1.png" class="zh-icon"></image>
			<input type="text" value="" placeholder="请输入密码" />
		</view>
		
		<view class="btn600 mt-1">确定</view>
		
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
.login-logo{width: 200rpx;height: 207rpx;margin: 100rpx auto;}
.zh-icon{width: 30rpx;height: 36rpx;margin-right: 20rpx;}
input{font-size: 26rpx;color: #999;}
.m-75{margin-left: 75rpx;margin-right: 75rpx;}
</style>
