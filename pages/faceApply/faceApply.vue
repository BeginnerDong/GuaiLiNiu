<template>
	<view>
		
		<view class="bg-white px-2">
			<view class="py-3">绑定手机号：{{userData.info.phone}}</view>
			<view class="py-3 bT-e1">门店：{{shopData.name}}</view>
			<view class="py-3 bT-e1">店长手机号：{{shopData.phone}}</view>
		</view>
		
		<view class="btnAuto applyBtn">提交申请</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shopData:{},
				userData:{}
			}
		},
		onLoad() {
			const self = this;
			self.shopData = uni.getStorageSync('shopData')
			self.userData = uni.getStorageSync('user_info')
		},
		methods: {
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.applyBtn{width: 600rpx;margin-top: 70%;}
</style>
