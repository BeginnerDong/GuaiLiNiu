<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">可用优惠券</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">不可用优惠券</view>
			</view>
		</view>
		
		<view>
			<block v-for="(item,index) in mainData" :key="index">
				<view class="mt-3 colorf p-r" v-if="navCurr==item.canUse" @click="choose(item)">
					<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
					<view class="p-aXY p-3 top-0 left-0 flex1">
						<view class="px-1 flex-1">
							<view class="font-40 pb-5 mb-3">满{{item.condition}}元减{{item.value}}元</view>
							<view class="font-22">有效期至：{{item.invalid_time_change}}</view>
						</view>
						<image src="../../static/images/coupons-icon2.png" class="wh60 mr-5"></image>
					</view>
				</view>
			</block>
			
			
			
		
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:0,
				standardPrice:0,
				mainData:[],
				searchItem:{
					thirdapp_id: 2
				},
				
			}
		},
		onLoad(options) {
			const self = this;
			self.standardPrice = parseFloat(options.standardPrice);
			console.log('self.standardPrice',self.standardPrice)
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			choose(item){
				const self = this;
				if(item.canUse==0){
					uni.setStorageSync('chooseCoupon',item);
					uni.navigateBack({
						delta:1
					});
				}
			},
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					'create_time':'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for(var i=0;i<self.mainData.length;i++){
							self.mainData[i].invalid_time_change = self.$Utils.timeto(parseInt(self.mainData[i].invalid_time),'ymd-hm')
							if(parseFloat(self.mainData[i]['condition'])<self.standardPrice){
								self.mainData[i]['canUse'] = 0;
							}else{
								self.mainData[i]['canUse'] = 1;
							}
						};
					};
					uni.setStorageSync('canClick', true);
					console.log('coupon',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userCouponGet(postData, callback);
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
	.li{width: 50%;}
.couponImg{width: 713rpx;height: 220rpx;margin: auto;}
</style>
