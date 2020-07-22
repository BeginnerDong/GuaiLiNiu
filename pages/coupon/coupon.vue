<template>
	<view>
		
		<view class="logoBox">
			<image src="../../static/images/coupons-img-01.png" class="logo"></image>
		</view>
		
		<view class="p-r">
			<image src="../../static/images/coupons-img-02.png" class="couImg"></image>
			
			<view class="p-aXY couBox">
				<view class="mt-3 colorf p-r">
					<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
					<view class="p-aXY p-3 top-0 left-0 flex1">
						<view class="px-1 flex-1">
							<view class="font-40 pb-5 mb-3">满400元减200元</view>
							<view class="font-22">有效期至：2019.02.20-2019.11.30</view>
						</view>
						<view class="font-30 colorM pr-5">立即分享</view>
					</view>
				</view>
				<view class="px-2 py-4">
					<view class="font-30 font-w">活动规则</view>
					<view class="font-26 pt-2 line-h-lg">
						1、规则
					</view>
				</view>
			</view>
		</view>
		
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

<style>
/* page{background-color: #FF633A;} */
</style>
<style scoped>
.logoBox{background-color: #FF633A;padding: 30rpx;padding-bottom: 180rpx;}
.logo{width: 350rpx;height: 345rpx;margin: 0 auto;}
.couImg{height: 799rpx;margin-top: -150rpx;}
.couponImg{width: 713rpx;height: 220rpx;margin: auto;}
.couBox{margin-top: 150rpx;}
</style>