<template>
	<view class="bg-white pt-2">
		
		<view class="shadowM px-2 mx-2 radius10">
			<view class="flex1 py-2 bB-f5">
				<image src="../../static/images/the order-img.png" class="wh180"></image>
				<view class="px-2 py-3 flex-1">
					<view class="font-30 font-w">{{mainData.title}}</view>
					<view class="font-24 py-2">{{mainData.coach[0].name}} | {{mainData.start_time}}~{{mainData.end_time}}</view>
					<view class="flex">
						<view class="tag" v-for="(c_item,c_index) of mainData.description" :key="c_index">{{c_item}}</view>
					</view>
				</view>
			</view>
			<view class="font-26 color6 py-3">课程有效期：15天 </view>
		</view>
		
		<view class="mx-2">
			<view class="py-4 flex1 bB-f5">
				<view>上课地点</view>
				<view class="color6">{{mainData.shopInfor.address}}</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>教练</view>
				<view class="color6">{{mainData.coach[0].name}}</view>
			</view>
			<view class="py-4 flex1 bB-f5">
				<view>课时套餐</view>
				<view class="color6">（￥{{mainData.price}}/{{mainData.score}}课时）</view>
			</view>
			<view class="flex py-4 font-24">
				<image src="../../static/images/the order-icon5.png" class="wh30 mr-2" v-if="isAgree"></image>
				<image src="../../static/images/the order-icon4.png" class="wh30 mr-2" v-else></image>
				<view><text @click="isShow('agree')">同意</text> <text class="colorB" @click="isShow">《怪力牛运动会员服务协议》</text></view>
			</view>
		</view>
		
		
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
			<view class="font-26">已预约0/{{mainData.standard}}人</view>
			<button class="carBtn" open-type="getUserInfo" @click="submit">立即预约</button>
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
				is_show:false,
				isAgree:false,
				mainData:{}
			}
		},
		onLoad(){
			const self = this;
			self.mainData = uni.getStorageSync('orderDetail');
			console.log('order',self.mainData.shopInfor,self.mainData.coach[0])
		},
		methods: {
			isShow(type){
				const self = this;
				if(type){
					self.isAgree = !self.isAgree;
				}else{
					self.is_show = !self.is_show
				}
			},
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				var orderList = []
				if(self.isAgree){
					orderList.push({product_id:self.mainData.id,count:1,type:1});
					const callback = (user, res) => {
						self.addOrder(orderList)
					};
					self.$Utils.getAuthSetting(callback);
				}else{
					uni.showModal({
						title:'',
						content:'请先同意会员服务协议',
						showCancel:false
					})
				}
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
						uni.setStorageSync('canClick', true);
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;
				const postData = {};
				console.log('self.mainData.price',self.mainData.price)
				postData.otherPay={
					price:parseFloat(self.mainData.price)
				};
				
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
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
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
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
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
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
					uni.setStorageSync('canClick', true);
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
.xy{height: 1000rpx;margin-top: 15%;}
.carBtn{border-radius: 0;}
</style>
