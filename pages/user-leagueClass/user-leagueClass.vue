<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">全部</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">待使用</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">进行中</view>
				<view class="li" :class="navCurr==3?'on':''" @click="changeNav(3)">待评价</view>
				<view class="li" :class="navCurr==4?'on':''" @click="changeNav(4)">已评价</view>
			</view>
		</view>
		<view class="f5Bj-H20 mb-3"></view>
		
		<view class="shadowM px-2 mx-2 mb-3 radius10 line-h">
			<view class="font-24 color6 flex1 py-3 bB-f5">
				<view>订单编号：15658989746565862</view>
				<view class="colorR">待使用</view>
				<!-- <view class="colorR">使用中</view> -->
				<!-- <view class="colorR">待评价</view> -->
				<!-- <view class="colorR">已评价</view> -->
			</view>
			<view class="flex1 py-3 bB-f5 w-100">
				<image src="../../static/images/the order-img.png" class="wh180"></image>
				<view class="px-2 flex-1 flex-1">
					<view class="font-30 font-w">减脂训练营·中上</view>
					<view class="pt-2"><text class="font-w price">220</text>/9课时</view>
					<view class="font-24 py-2">Auger | 20:00~20:45</view>
					<view class="flex">
						<view class="tag tagY">矫正</view>
						<view class="tag tagB">提升柔软度</view>
						<view class="tag tagG">改善身体线条</view>
					</view>
				</view>
			</view>
			<view class="font-26 color6 py-3 bB-f5">课程有效期：15天 </view>
			<!-- 其他 -->
			<view class="py-3 d-flex j-end">
				<view class="btn b-e1" @click="Router.navigateTo({route:{path:'/pages/user-appointCoach/user-appointCoach'}})">立即使用</view>
				<view class="btn b-e1" @click="Router.navigateTo({route:{path:'/pages/user-comment/user-comment?type=0'}})">立即评价</view>
				<view class="btn b-e1" @click="Router.navigateTo({route:{path:'/pages/user-commentDetail/user-commentDetail?type=0'}})">查看评价</view>
			</view>
			
			<!-- 进行中 -->
			<!-- <view class="py-3 flex1">
				<view>核销码：</view>
				<image src="../../static/images/my class-img.png" class="wh80"></image>
			</view> -->
		</view>
		
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

<style scoped>
.li{width: 20%;}
.btn{width: 160rpx;line-height: 60rpx;text-align: center;border-radius: 5rpx;}
</style>
