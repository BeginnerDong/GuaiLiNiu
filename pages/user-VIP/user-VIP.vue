<template>
	<view class="h-100 flexY bT-e1">
		
		<view class="flex-1 d-flex">
			<view class="left bR-e1 h-100 bg-f5">
				<view class="py-3 bB-e1" :class="leftCurr==0?'on':''" @click="changeLeft(0)">
					<view>今天</view>
					<view class="font-20 color9 date">周日</view>
				</view>
				<view class="py-3 bB-e1" :class="leftCurr==1?'on':''" @click="changeLeft(1)">
					<view>明天</view>
					<view class="font-20 color9 date">周日</view>
				</view>
			</view>
			
			<view class="flex-1 h-100 flexY right">
				
				<view class="d-flex flex-wrap">
					<view class="flex py-2" v-for="v in 5">
						<view class="time">8:00</view>
						<image src="../../static/images/about-img1.png" class="wh80 mx-2"></image>
						<view>
							<view>张少娜</view>
							<view class="font-26">15684278935</view>
						</view>
					</view>
					<view class="flex py-2">
						<view class="time">12:00</view>
						<image src="../../static/images/about-img1.png" class="wh80 mx-2"></image>
						<view>
							<view>张少娜</view>
							<view class="font-26">15684278935</view>
						</view>
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
				leftCurr:0
			}
		},
		methods: {
			changeLeft(i){
				const self = this;
				self.leftCurr = i;
			}
		}
	}
</script>
<style>
page{height: 100%;}
</style>
<style scoped>
.left{width: 140rpx;text-align: center;}
.left .on{background-color: #fff;color: #FF633A;position: relative;}
.left .on::before{content: '';background-color: #FF633A;width: 8rpx;height: 100%;border-top-right-radius: 8rpx;border-bottom-right-radius: 8rpx;position: absolute;top: 0;left: 0;}
.left .on .date{color: #FF633A;}
.right{padding: 30rpx 30rpx 80rpx;}
.time{width: 80rpx;text-align: center;}
</style>
