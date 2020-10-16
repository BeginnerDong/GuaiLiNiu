<template>
	<view>
		
		<view class="m-2 p-r mb-0 bg-white">
			<image src="../../static/images/preferential-icmg2.png" class="VIPcard"></image>
			<view class="font-24 colorf flex1 p-a bottom-0 left-0 right-0 p-3">
				<view>
					<view>{{activityName}}</view>
					<view>仅限门店{{shopData.name}}</view>
				</view>
				<!-- <view class="colorM">
					<view class="font-w">VIP</view>
					<view>年卡</view>
				</view> -->
			</view>
		</view>
		<view class="px-2 bg-white">
			<view class="py-4 flex1 bB-f5">
				<view>价格</view>
				<view class="price">{{mainData.price}}</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>有效期</view>
				<view class="color6">{{mainData.duration}}天</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>仅限门店</view>
				<view class="color6">{{shopData.name}}</view>
			</view>
		</view>
		
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
			<view class="font-2 d-flex a-center">总计： <text class="colorR font-36">{{mainData.price}}</text></view>
			<button class="carBtn" size="mini" open-type="getUserInfo"
			@getuserinfo="Router.navigateTo({route:{
				path:'/pages/VIP-information/VIP-information?product_id='
				+product_id
				+'&&price='
				+price
				+'&&duration='
				+duration}})
				">确认支付</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				shopData:{},
				activityName:'',
				product_id:'',
				price:'',
				duration:''
			}
		},
		onLoad(option){
			const self = this;
			self.activityName = option.name;
			self.mainData = JSON.parse(option.card);
			console.log(self.mainData)
			self.product_id = self.mainData.id;
			self.price = self.mainData.price;
			self.duration = self.mainData.duration
			self.shopData = uni.getStorageSync('shopData');
		},
		methods: {
			
			
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.VIPcard{width: 100%;height: 300rpx;}
.carBtn{height: 100rpx;border-radius: 0;line-height: 100rpx!important;}
</style>
