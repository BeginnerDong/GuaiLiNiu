<template>
	<view>
		
		<view class="logoBox">
			<image src="../../static/images/coupons-img-01.png" class="logo"></image>
		</view>
		
		<view class="p-r">
			<image src="../../static/images/coupons-img-02.png" class="couImg"></image>
			
			<view class="p-aXY couBox">
				<view class="mt-3 colorf p-r">
					<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
					<view class="p-aXY p-3 top-0 left-0 flex1">
						<view class="px-1 flex-1">
							<view class="font-40 pb-5 mb-3">满{{couponData.condition}}元减{{couponData.value}}元</view>
							<view class="font-22">有效期至：{{couponData.valid_time}}天</view>
						</view>
						<view class="font-30 colorM pr-5" @click="submit()">领取优惠卷</view>
					</view>
				</view>
				<view class="px-2 py-4">
					<view class="font-30 font-w">活动规则</view>
					<view class="font-26 pt-2 line-h-lg">
						{{couponData.description}}
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
				mainData:{},
				newData:{
					coupon_no:'',
					parent_no:''
				},
				couponData:{}
				
			}
		},
		onLoad(options) {
			const self = this;
			self.newData.coupon_no = options.coupon_no;
			self.newData.parent_no = options.parent_no;
			uni.setStorageSync('getProjectTokenParams',self.newData);
			self.$Utils.loadAll(['getUserData','getCouponData'], self);
			
		},
		methods: {
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserData');
				};
				console.log('postdata',postData);
				self.$apis.userGet(postData, callback);
			},
			getCouponData(){
				const self = this;
				const postData = {};
				postData.searchItem = {
					coupon_no:self.newData.coupon_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData =  res.info.data[0];
					};
					uni.setStorageSync('canClick', true);
					console.log('coupon',self.mainData)
					self.$Utils.finishFunc('getCouponData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
			submit(item){
				const self = this;
				// self.couponData = item;
				uni.setStorageSync('canClick', false);
				var couponList = []
				
				couponList.push({
					coupon_id:self.couponData.id,
					count:1,
					type:1,
					data:{
						course_type:self.couponData.course_type
					}
				});
				self.addOrder(couponList)
			},
			
			addOrder(couponList) {
				const self = this;	
				const postData = {}; 
				postData.couponList = self.$Utils.cloneForm(couponList);
				postData.tokenFuncName = 'getProjectToken';
				console.log('addOrder',postData);
				const callback = (res) => {
					
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						// self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
						uni.setStorageSync('canClick', true);
					};		
				};
				self.$apis.addCoupon(postData, callback);
			},
		}
	}
</script>

<style>
/* page{background-color: #FF633A;} */
</style>
<style scoped>
.logoBox{background-color: #FF633A;padding: 30rpx;padding-bottom: 180rpx;}
.logo{width: 350rpx;height: 345rpx;margin: 0 auto;}
.couImg{height: 799rpx;margin-top: -150rpx;}
.couponImg{width: 713rpx;height: 220rpx;margin: auto;}
.couBox{margin-top: 150rpx;}
</style>