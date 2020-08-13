<template>
	<view>
		
		<view class="font-24 flex1 py-3 px-2 bB-f5 bg-white store"
		v-for="(item,index) in mainData" :key="index" @click="goBack(item)">
			<image src="../../static/images/yuyue-icon-01.png" class="wh36 mr-2" v-if="shopId==item.id"></image>
			<image src="../../static/images/the-order-icon4.png" class="wh36 mr-2" v-else></image>
			<view class="flex-1 flex1">
				<image :src="item.mainImg[0].url" class="storeImg"></image>
				<view class="flex3 pl-2 flex-1">
					<view class="font-32 font-w">{{item.name}}</view>
					<view class="color6 avoidOverflow2 dz">{{item.address}}</view>
					<view>距离：2.23KM</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:[],
				shopId:''
			}
		},
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('shopList')
			self.shopId = uni.getStorageSync('shopData').id;
		},
		methods: {
			
			goBack(item){
				const self = this;
				uni.setStorageSync('shopData',item);
				uni.navigateBack({
				    delta: 1
				});
			},
			
		}
	}
</script>
<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.storeImg{width: 230rpx;height: 160rpx;border-radius: 10rpx;}
.store .dz{width: 400rpx;padding: 15rpx 0;}
</style>
