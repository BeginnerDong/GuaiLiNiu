<template>
	<view>
		
		<view class="bg-white pt-2">
			<view class="shadowM px-2 mx-2 radius10">
				<view class="flex1 py-2 bB-f5">
					<image :src="mainData&&mainData.mainImg&&mainData.mainImg[0]&&mainData.mainImg[0].url" class="wh180 radius20"></image>
					<view class="px-2 py-3 flex-1">
						<view class="font-30 font-w pb-1">{{mainData.title}}</view>
						<view><text class="price font-w">{{mainData.price}}</text>/{{mainData.score}}课时</view>
						<view class="font-24 py-1 line-h-sm">{{mainData.coach[0].name}} | {{mainData.book_week_item}}~{{mainData.book_time_item}}</view>
						<view class="flex">
							<block v-for="(c_item,c_index) in mainData.description_change" :key="c_index">
								<view class="tag">{{c_item}}</view>
							</block>
						</view>
					</view>
				</view>
				<view class="font-26 color6 py-3">课程有效期：{{mainData.duration}}天 </view>
			</view>
			
			<view class="mx-2">
				<view class="py-4 flex1 bB-f5">
					<view>上课地点</view>
					<view class="color6">{{shopData.name}}</view>
				</view>
				
				<view class="py-4 flex1 bB-f5">
					<view>教练</view>
					<view class="color6">{{mainData.coach[0].name}}</view>
				</view>
				
				<view class="py-4 bB-f5">
					<view class="pb-4 flex1">
						<view>课时套餐（￥{{mainData.price}}/{{mainData.score}}课时）</view>
						<view class="flex">
							<image src="../../static/images/the-order-icon.png" class="wh40" @click="count(-1)"></image>
							<view class="num">{{num}}</view>
							<image src="../../static/images/the-order-icon1.png" class="wh40" @click="count(1)"></image>
						</view>
					</view>
					<!-- <view class="flex1 tcBox">
						<view class="tc" :class="tcCurr==0?'on':''" @click="changeCurr(0)">30节套餐</view>
						<view class="tc" :class="tcCurr==1?'on':''" @click="changeCurr(1)">50节套餐</view>
						<view class="tc" :class="tcCurr==2?'on':''" @click="changeCurr(2)">80节套餐</view>
						<view class="tc" :class="tcCurr==3?'on':''" @click="changeCurr(3)">100节套餐</view>
					</view> -->
				</view>
				
				<view class="py-4 flex1 bB-f5">
					<view>优惠券</view>
					<view class="flex" @click="Router.navigateTo({route:{path:'/pages/payLeagueClass-coupon/payLeagueClass-coupon?standardPrice='+mainData.price}})">
						<view class="color6">
							{{chooseCoupon.id?'满'+chooseCoupon.snap_coupon.condition+'减'+chooseCoupon.snap_coupon.value:'无使用'}}
						</view>
						<image src="../../static/images/the-order-icon3.png" class="R-icon ml-1"></image>
					</view>
				</view>
				
				<view class="py-4 flex1 bB-f5">
					<view>支付方式</view>
					<view class="flex">
						<image src="../../static/images/the-order-icon2.png" class="wh44 mr-1"></image>
						<view class="color6">微信支付</view>
					</view>
				</view>
				
				<view class="flex py-4 font-24">
					<image src="../../static/images/the-order-icon5.png" class="wh30 mr-2" v-if="isAgree"></image>
					<image src="../../static/images/the-order-icon4.png" class="wh30 mr-2" v-else></image>
					<view><text @click="isShow('agree')">同意</text> <text class="colorB" @click="isShow()">《怪力牛运动会员服务协议》</text></view>
				</view>
			</view>
		</view>
		
		
		<view style="height: 130rpx;"></view>
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
			<view class="font-26">总计：<text class="colorR font-36 price">{{totle}}</text></view>
			
			<button class="carBtn" open-type="getUserInfo" @getuserinfo="submit" >立即预约</button>
		</view>
		
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white radius20 mx-4 flexY xy">
				<view class="font-30 text-center py-3">《怪力牛会员购买服务协议》</view>
				<view class="px-3 mb-3 flex-1 flexY">
					1、都必须为为和促进OK了
				</view>
				<view class="text-center colorf py-3 Mgb" @click="isShow()">确定</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				tcCurr:0,
				mainData:{},
				num:1,
				is_show:false,
				isAgree:false,
				chooseCoupon:{},
				shopData:{},
				totle:0
			}
		},
		onLoad(){
			const self = this;
			uni.removeStorageSync('chooseCoupon');
			self.mainData = uni.getStorageSync('sijiaoCourseDetail');
			self.shopData = uni.getStorageSync('shopData');
			self.num = self.mainData.score;
		},
		onShow(){
			const self = this;
			if(uni.getStorageSync('chooseCoupon')){
				self.chooseCoupon = uni.getStorageSync('chooseCoupon')
			}
			console.log('self.chooseCoupon',self.chooseCoupon)
			self.totlePrice();
		},
		methods: {
			
			count(x){
				const self = this;
				if(self.num+x < self.mainData.score){
					return;
				}else{
					self.num = self.num+x;
				}
				self.totlePrice();
			},
			totlePrice(){
				const self = this;
				self.totle = parseFloat(self.mainData.price ) * self.num
			},
			
			isShow(type){
				const self = this;
				if(type){
					self.isAgree = !self.isAgree;
				}else{
					self.is_show = !self.is_show
				}
			},
			changeCurr(i){
				const self = this;
				self.tcCurr = i
			},
			
			successSubmit(){
				const self = this;
				self.Utils.stopMultiClick(self.submit)
			},
			
			submit(){
				const self = this;
				// uni.setStorageSync('canClick', false);
				var orderList = []
				if(self.isAgree){
					orderList.push({
						product_id:self.mainData.id,
						count:self.num,
						type:1,
						data:{
							course_type:self.mainData.course_type,
							coach_no:self.mainData.coach_no,
							shop_no:self.mainData.shop_no,
						}
				});
					self.addOrder(orderList)
				}else{
					uni.showModal({
						title:'',
						content:'请先同意会员服务协议',
						showCancel:false
					})
				}
				uni.setStorageSync('canClick', true);
			},
			
			addOrder(orderList) {
				const self = this;	
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.tokenFuncName = 'getProjectToken';
				console.log('addOrder',postData);
				const callback = (res) => {
					
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
						// uni.setStorageSync('canClick', true);
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;
				const postData = {};
				console.log('price',parseFloat(self.mainData.price)*self.num)
				
				if(uni.getStorageSync('chooseCoupon')){
					postData.couponPay = [{
						id:uni.getStorageSync('chooseCoupon').id,
						price:parseFloat(uni.getStorageSync('chooseCoupon').value)
					}];
					postData.wxPay = {
						price:(self.totle - parseFloat(uni.getStorageSync('chooseCoupon').value)).toFixed(2) 
					};
				}else{
					postData.wxPay = {
						price:self.totle
					};
				};
				console.log('postData',postData)
				
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [];
				postData.payAfter.push({
					tableName: 'Order',
					FuncName: 'update',
					data: {
						standard:self.mainData.score
					},
					searchItem:{
						id:self.orderId
					}
				});
				
				const callback = (res) => {
					if (res.solely_code == 100000) {
						// uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData);
								uni.removeStorageSync('chooseCoupon');
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user/user'}})
									}, 1000);
								} else {
									// uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							uni.removeStorageSync('chooseCoupon');
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user/user'}})
							}, 1000);
						};
					} else {
						// uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
					// uni.setStorageSync('canClick', true);
				};
				self.$apis.pay(postData, callback);
			},
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.colorB{color: #63D1F8;}
.tc{font-size: 24rpx;line-height: 80rpx;text-align: center;border: 1px solid #e1e1e1;border-radius: 10rpx;width: 155rpx;color: #666;}
.tcBox .on{border: 1px solid #FF633A;background: #FFEEE9;color: #000000;}
.carBtn{border-radius: 0;}
.xy{height: 1000rpx;margin-top: 15%;}
</style>
