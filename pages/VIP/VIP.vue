<template>
	<view>
		
		<view class="vipHead colorf font-24 line-h px-2">
			<view class="flex py-3 font-30 font-w" @click="Show()">
				<view>西安外事学院店</view>
				<image src="../../static/images/members-icon.png" class="sj-icon"></image>
			</view>
			<view>距离 2.22KM</view>
			<view class="flex1 py-3">
				<view>西安市雁塔区鱼斗路左岸春天A座2025</view>
				<image src="../../static/images/members-icon1.png" class="homeDz-icon"></image>
			</view>
			
			<view class="p-r color2">
				<image src="../../static/images/members-img1.png" class="vipBg"></image>
				<view class="p-aX top-0 px-5 py-3">
					<view class="flex font-30 font-w">
						<image src="../../static/images/members-icon2.png" class="vip-icon"></image>
						<view v-if="vip==0">VIP会员</view>
						<view v-else>会员码：56847528</view>
					</view>
					<view class="pt-4 font-24" v-if="vip==0">享受更多的优惠权益</view>
					<view class="pt-4 font-24" v-else>有效期：2020.06.23-2021.06.23</view>
				</view>
			</view>
		</view>
		
		<view class="p-r line-h">
			<image src="../../static/images/members-img2.png" class="vipBg1 m100"></image>
			
			<view class="p-aXY ">
				<view v-show="vip==0">
					<view class="mt-2 px-2">
						<view class="font-w py-4">选择套餐</view>
						<view class="flex1 mb-4 tcBox">
							<view class="tc b-e1 p-r radius10" 
							v-for="(item,index) in mainData"
							:class="tcCurr==index?'on':''" @click="changeTc(index,item)">
								<view class="font-24 pb-3">{{item.title}}</view>
								<view class="price font-32">{{item.price}}</view>
								<image src="../../static/images/members-icon3.png" class="yes" v-show="tcCurr==index"></image>
							</view>
						</view>
					</view>
					<view class="f5Bj-H20"></view>
				</view>
				
				<view class="">
					<view class="font-w py-4 px-2">会员权益</view>
					<view class="flex mb-4 flex-wrap font-24 vipBox">
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon4.png" class="memberIcon"></image>
							<view>器械不限用</view>
						</view>
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon5.png" class="memberIcon"></image>
							<view>到店体验服务</view>
						</view>
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon6.png" class="memberIcon"></image>
							<view>团课免费约</view>
						</view>
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon7.png" class="memberIcon"></image>
							<view>7*24营业</view>
						</view>
					</view>
					
					<view class="btnAuto" v-show="vip==1"><text class="price">999</text>/年卡 立即续费</view>
				</view>
			</view>
		</view>
		
		<view v-show="vip==0">
			<view style="height: 130rpx;"></view>
			<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
				<view class="font-24"><text class="price font-w font-36">299</text>/12个月</view>
				<view class="carBtn" @click="submit">确认支付</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tcCurr:0,
				vip:0,
				mainData:[],
				product_id:0,
				searchItem:{
					thirdapp_id: 2,
					type: 2,
				},
				Router:this.$Router
			}
		},
		onLoad(option){
			const self = this;
			self.searchItem.user_no = uni.getStorageSync('shopData')['user_no'];
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			changeTc(i,item){
				const self = this;
				self.tcCurr = i;
				self.product_id = item.id;
				
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				wx.requestSubscribeMessage({
				  tmplIds: [
					  'BZ74KvhYwLWYKzcY-OunKaWIkbsBy_wWZ01LaZsGlKo',
					  'qAz8Vn3an8LzQ52VplqDpKsoUTCQHM7E9g3Cd4opzvo',
					],
				  success (res) {console.log(res) }
				});
				return;
				var orderList = []
				orderList.push({product_id:self.product_id,count:1,type:1});
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
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
				postData.wxPay = {
					price:parseFloat(self.mainData.price)
				};
					
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				/* postData.payAfter = [];
				postData.payAfter.push({
					tableName: 'UserInfo',
					FuncName: 'update',
					data: {
						standard:self.mainData.score
					},
					searchItem:{
						id:self.orderId
					}
				}); */
				
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
											uni.removeStorageSync('user_token');
											uni.removeStorageSync('token_expire_time');
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
.vipHead{background: linear-gradient(to bottom, #A47B4C 20%,#B3916B 50%);}
.vipBg1{height: 859rpx;}
.m100{margin-top: -100rpx;}
.tc{width: 220rpx;height: 140rpx;text-align: center;line-height: 1;padding-top: 30rpx;}
.tcBox .on{background-color: #FFF5F2;border: 1px solid #FF633A;}
.yes{width: 61rpx;height: 51rpx;position: absolute;bottom: 0;right: 0;}

.memberIcon{width: 48rpx;height: 53rpx;margin-bottom: 30rpx;}
.vipItem{width: 25%;margin-bottom: 30rpx;}

.btnAuto{width: 710rpx;margin-top: 200rpx;}
.btnAuto .price{color: #fff;}
</style>
