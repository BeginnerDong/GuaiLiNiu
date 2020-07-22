<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">可用优惠券</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">不可用优惠券</view>
			</view>
		</view>
		
		<view>
			<view class="mt-3 colorf p-r" v-show="navCurr==0">
				<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
				<view class="p-aXY p-3 top-0 left-0 flex1">
					<view class="px-1 flex-1">
						<view class="font-40 pb-5 mb-3">满400元减200元</view>
						<view class="font-22">有效期至：2019.02.20-2019.11.30</view>
					</view>
					<image src="../../static/images/coupons-icon2.png" class="wh60 mr-5"></image>
				</view>
			</view>
			
			
			<view class="mt-3 colorf p-r" v-show="navCurr==1">
				<image src="../../static/images/coupons-icon1.png" class="couponImg"></image>
				<view class="p-aXY p-3 top-0 left-0 flex1">
					<view class="px-1 flex-1">
						<view class="font-40 pb-5 mb-3">满400元减200元</view>
						<view class="font-22">有效期至：2019.02.20-2019.11.30</view>
					</view>
					<view class="font-30 color9 pr-5">不可使用</view>
				</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:0
			}
		},
		methods: {
			changeNav(i){
				const self = this;
				self.navCurr = i
			}
		}
	}
</script>
<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
	.li{width: 50%;}
.couponImg{width: 713rpx;height: 220rpx;margin: auto;}
</style>
