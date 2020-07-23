<template>
	<view class="bg-white pt-2">
		
		<view class="shadowM px-2 mx-2 radius10">
			<view class="flex1 py-2 bB-f5">
				<image src="../../static/images/the order-img.png" class="wh180"></image>
				<view class="px-2 py-3 flex-1">
					<view class="font-30 font-w">减脂训练营·中上</view>
					<view class="font-24 py-2">Auger | 20:00~20:45</view>
					<view class="flex">
						<view class="tag tagY">矫正</view>
						<view class="tag tagB">提升柔软度</view>
						<view class="tag tagG">改善身体线条</view>
					</view>
				</view>
			</view>
			<view class="font-26 color6 py-3">课程有效期：15天 </view>
		</view>
		
		<view class="mx-2">
			<view class="py-4 flex1 bB-f5">
				<view>上课地点</view>
				<view class="color6">西安外事学院店</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>教练</view>
				<view class="color6">张丹</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>课时套餐</view>
				<view class="color6">（￥200/9课时）</view>
			</view>
			<view class="flex py-4 font-24">
				<image src="../../static/images/the order-icon4.png" class="wh30 mr-2"></image>
				<!-- <image src="../../static/images/the order-icon5.png" class="wh30 mr-2"></image> -->
				<view>同意 <text class="colorB" @click="isShow">《怪力牛运动会员服务协议》</text></view>
			</view>
		</view>
		
		
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
			<view class="font-26">已预约8/12人</view>
			<view class="carBtn">立即预约</view>
		</view>
		
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white radius20 mx-4 flexY xy">
				<view class="font-30 text-center py-3">《怪力牛会员购买服务协议》</view>
				<view class="px-3 mb-3 flex-1 flexY">
					1、都必须为为和促进OK了
				</view>
				<view class="text-center colorf py-3 Mgb" @click="isShow">确定</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				is_show:false
			}
		},
		methods: {
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			}
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.colorB{color: #63D1F8;}
.xy{height: 1000rpx;margin-top: 15%;}
</style>
