<template>
	<view>
		
		<view class="m-2 p-r mb-0 bg-white">
			<image src="../../static/images/preferential-icmg2.png" class="VIPcard"></image>
			<view class="font-24 colorf flex1 p-a bottom-0 left-0 right-0 p-3">
				<view>
					<view>活动年卡</view>
					<view>仅限门店西安</view>
				</view>
				<view class="colorM">
					<view class="font-w">VIP</view>
					<view>年卡</view>
				</view>
			</view>
		</view>
		<view class="px-2 bg-white">
			<view class="py-4 flex1 bB-f5">
				<view>价格</view>
				<view class="price">799</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>有效期</view>
				<view class="color6">一年</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>仅限门店</view>
				<view class="color6">西安外事学院店</view>
			</view>
		</view>
		
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
			<view class="font-2 d-flex a-center">总计： <text class="colorR font-36">799</text></view>
			<view class="carBtn">确认支付</view>
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
	page{background-color: #f5f5f5;}
.VIPcard{width: 100%;height: 300rpx;}
</style>
