<template>
	<view class="h-100 flexY">
		
		<view class="flex1 py-3 px-2 bB-e1 w-100">
			<image src="../../static/images/the order-img.png" class="wh120"></image>
			<view class="px-2 flex5 flex-1 h-120">
				<view class="font-30 font-w flex-1">减脂训练营·中上</view>
				<view class="flex">
					<view class="tag tagY">矫正</view>
					<view class="tag tagB">提升柔软度</view>
					<view class="tag tagG">改善身体线条</view>
				</view>
			</view>
		</view>
		
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
					<view class="jl color6 shadowM p-r mt-5" v-for="v in 5">
						<view>8:00</view>
						<!-- <image src="../../static/images/yuyue-icon-01.png" class="wh26"></image> -->
						<image src="../../static/images/yuyue-icon-02.png" class="wh26"></image>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="bT-e1 p-2">
			<view class="btnAuto">确定</view>
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
.h-120{height: 120rpx;}
.btnAuto{width: 710rpx;}

.left{width: 140rpx;text-align: center;}
.left .on{background-color: #fff;color: #FF633A;}
.left .on .date{color: #FF633A;}
.right{padding: 30rpx 30rpx 80rpx;}
.jl{margin-right: 68rpx;width: 140rpx;line-height: 80rpx;text-align: center;}
.jl:nth-child(3n){margin-right: 0;}

.wh26{position: absolute;top: -10rpx;right: -10rpx;}
</style>
