<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">大厅</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">未使用</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">已使用</view>
				<view class="li" :class="navCurr==3?'on':''" @click="changeNav(3)">已过期</view>
			</view>
		</view>
		
		<view>
			<view class="mt-3 colorf p-r" v-show="navCurr==0" v-for="(item,index) in mainData">
				<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
				<view class="p-aXY p-3 top-0 left-0 flex1">
					<view class="px-1 flex-1">
						<view class="font-40 pb-5">满{{item.condition}}元减{{item.value}}元</view>
						<view class="font-22">
							<view>有效期：{{item.valid_time}}天</view>
							<view>有效期至：2019.02.20-2019.11.30</view>
						</view>
					</view>
					<view class="font-30 colorM pr-5">立即领取</view>
				</view>
			</view>
			
			<view class="mt-3 colorf p-r" v-show="navCurr==1">
				<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
				<view class="p-aXY p-3 top-0 left-0 flex1">
					<view class="px-1 flex-1">
						<view class="font-40 pb-5 mb-3">满400元减200元</view>
						<view class="font-22">有效期至：2019.02.20-2019.11.30</view>
					</view>
					<image src="../../static/images/coupons-icon2.png" class="wh60 mr-5"></image>
				</view>
			</view>
			
			
			<view class="mt-3 colorf p-r" v-show="navCurr==2">
				<image src="../../static/images/coupons-icon1.png" class="couponImg"></image>
				<view class="p-aXY p-3 top-0 left-0 flex1">
					<view class="px-1 flex-1">
						<view class="font-40 pb-5 mb-3">满400元减200元</view>
						<view class="font-22">有效期至：2019.02.20-2019.11.30</view>
					</view>
					<view class="font-30 color9 pr-5">不可使用</view>
				</view>
			</view>
			
			<view class="mt-3 colorf p-r" v-show="navCurr==3">
				<image src="../../static/images/coupons-icon1.png" class="couponImg"></image>
				<view class="p-aXY p-3 top-0 left-0 flex1">
					<view class="px-1 flex-1">
						<view class="font-40 pb-5 mb-3">满400元减200元</view>
						<view class="font-22">有效期至：2019.02.20-2019.11.30</view>
					</view>
					<view class="font-30 color9 pr-5">已经过期</view>
				</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:0,
				mainData:[],
				searchItem:{
					thirdapp_id: 2
				}
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					'create_time':'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							// self.mainData[i].description = self.mainData[i].description.split(',');
							// self.mainData[i].start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'hm')
							self.mainData[i].end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'hm')
						}
					};
					uni.setStorageSync('canClick', true);
					console.log('coupon',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
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
.li{width: 25%;}
.couponImg{width: 713rpx;height: 220rpx;margin: auto;}
.lq{width: 160rpx;line-height: 50rpx;border-radius: 10rpx;text-align: center;margin-right: 40rpx;}
</style>
