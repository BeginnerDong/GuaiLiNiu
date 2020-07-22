<template>
	<view>
		
		<view class="p-r z10 bg-white p-s mb-2 top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">全部</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">已预约</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">已完成</view>
			</view>
		</view>
		
		<view class="bg-white px-2 mx-2 mb-2 radius10 line-h p-r">
			<view class="colorf sgin">已完成</view>
			<view class="flex1 py-3 bB-f5 w-100">
				<image src="../../static/images/the order-img.png" class="img"></image>
				<view class="px-2 py-2 flex-1 d-flex flex-column j-sb h-180">
					<view class="font-30 font-w">减脂训练营·中上</view>
					<view class="flex">
						<view class="tag tagY">减脂塑形</view>
						<view class="tag tagB">肌肉塑形</view>
					</view>
					<view class="colorR"><text class="price">220</text>/课时</view>
				</view>
			</view>
			<view class="py-3 bB-f5 flex1">
				<view>预约时间</view>
				<view>2020.06.23 14:00</view>
			</view>
			<view class="py-3">
				<view>预约学员（10）</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/about-img1.png" class="wh80" v-for="v in 10"></image>
				</view>
			</view>
		</view>
		
		<image src="../../static/images/sijiao yuyue-img2.png" class="wh100 p-f"></image>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
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
.li{width: 33.33%;}
.img{width: 200rpx;height: 160rpx;}
.wh80{margin-right: 39rpx;margin-top: 20rpx;}
.wh80:nth-child(6n){margin-right: 0;}
.h-180{height: 180rpx;}
.sgin{line-height: 40rpx;background-color: #FF633A;position: absolute;border-top-left-radius: 20rpx;border-bottom-left-radius: 20rpx; padding: 0 15rpx;right: 0;top: 20rpx;}
.wh100{top: 70%;right: 0;}
</style>
