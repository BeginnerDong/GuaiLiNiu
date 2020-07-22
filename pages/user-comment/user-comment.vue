<template>
	<view>
		
		<view class="shadowM px-2 mx-2 mt-3 bg-white radius10 line-h">
			<view class="font-24 color6 flex1 py-3 bB-f5">
				<view>订单编号：15658989746565862</view>
				<view class="colorR">待评价</view>
			</view>
			<view class="flex1 py-3 bB-f5 w-100">
				<image src="../../static/images/the order-img.png" class="wh180"></image>
				<view class="px-2 flex-1 d-flex flex-column j-sb h-180">
					<view class="font-30 font-w">减脂训练营·中上</view>
					
					<!-- 团课评价显示 -->
					<view v-show="type==0"><text class="font-w price">220</text>/9课时</view>
					<view class="font-24" v-show="type==0">Auger | 20:00~20:45</view>
					
					<view class="flex">
						<view class="tag tagY">矫正</view>
						<view class="tag tagB">提升柔软度</view>
						<view class="tag tagG">改善身体线条</view>
					</view>
					
					<!-- 私教课评价显示 -->
					<view class="colorR" v-show="type==1"><text class="price">220</text>/课时</view>
				</view>
			</view>
			<view class="font-26 color6 py-3 bB-f5">课程有效期：15天 </view>
		</view>
		
		<view class="shadowM bg-white mx-2 mt-2 px-2 py-3 radius10">
			<textarea value="" placeholder="请填写评论,让更多人知道" class="comment"/>
			<view class="flex font-22 color9 flex-wrap">
				<image src="../../static/images/my class-img1.png" class="wh100 mr-2"></image>
				
				<image src="../../static/images/evaluation-img.png" class="wh100 mr-2"></image>
				<view>可上传多张图片</view>
			</view>
		</view>
		
		<view class="btnAuto">确定</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:0
			}
		},
		onLoad(option){
			const self = this;
			self.type = option.type;
		},
		methods: {
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.h-180{height: 180rpx;}
.comment{width: 710rpx;height: 275rpx;color: #999;font-size: 24rpx;}
.btnAuto{width: 710rpx;margin-top: 20rpx;}
</style>
