<template>
	<view class="px-2 font-30">

		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members-information-icon.png" class="inforIcon"></image>
			<view class="flex-1 pl-2">姓名</view>
			<input type="text" v-model="mainData.name" placeholder="请输入" />
		</view>
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members-information-icon1.png" class="inforIcon1"></image>
			<view class="flex-1 pl-2">年龄</view>
			<input type="text" v-model="mainData.birthday" placeholder="请输入" />
		</view>
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members-information-icon2.png" class="inforIcon2"></image>
			<view class="flex-1 pl-2">性别</view>
			<view class="font-26 flex">
				<view class="flex pl-5" @click="mainData.gender=0">
					<image v-if="mainData.gender==0" src="../../static/images/members-information-icon4.png" class="wh32 mr-1"></image>
					<image v-else src="../../static/images/members-information-icon5.png" class="wh32 mr-1"></image>
					<view>男</view>
				</view>
				<view class="flex pl-5" @click="mainData.gender=1">
					<image v-if="mainData.gender==1" src="../../static/images/members-information-icon4.png" class="wh32 mr-1"></image>
					<image v-else src="../../static/images/members-information-icon5.png" class="wh32 mr-1"></image>
					<view>女</view>
				</view>
			</view>
		</view>
		<view class="flex1 py-4 bB-f5">
			<image src="../../static/images/members-information-icon3.png" class="inforIcon"></image>
			<view class="flex-1 pl-2">电话</view>
			<input type="text" v-model="mainData.phone" placeholder="请输入" />
		</view>
		<view class="flex py-4 font-24" @click="isShow('agree')">
			<image src="../../static/images/the-order-icon5.png" class="wh30 mr-2" v-if="isAgree"></image>
			<image src="../../static/images/the-order-icon4.png" class="wh30 mr-2" v-else></image>
			<view><text>同意</text> <text class="colorB" @click="isShow()">《{{serviceData.title}}》</text></view>
		</view>
		<!-- <button class="carBtn" open-type="getUserInfo" @getuserinfo="submit" >立即预约</button> -->
		<button class="btnAuto" @click="successSubmit">确认</button>
		
		
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white radius20 mx-4 flexY xy">
				<view class="font-30 text-center py-3">{{serviceData.title}}</view>
				<view class="px-3 mb-3 flex-1 flexY">
					<view v-html="serviceData.content"></view>
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
				product_id: 0,
				Utils:this.$Utils,
				mainData: {
					name: '',
					gender: 0,
					birthday: '',
					phone: '',
					deadline: 0
				},
				serviceData:{},
				is_show:false,
				isAgree:false
			}
		},
		onLoad(options) {
			const self = this;
			self.product_id = options.product_id;
			console.log('options', options);
			self.getUserData();
			self.getServiceData();
			console.log('(new Date()).toLocaleDateString()', (new Date()).toLocaleDateString());
			console.log('self.mainData', self.mainData);
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
			
			getServiceData(){
				const self = this;
				const postData = {};
				postData.searchItem = {
					menu_id: 5,
					title:'怪力牛会员卡协议'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.serviceData =  res.info.data[0];
					};
					uni.setStorageSync('canClick', true);
					console.log('服务',self.serviceData)
					self.$Utils.finishFunc('getServiceData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getCardData() {
				const self = this;
				const postData = {};
			
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					 id:self.product_id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.cardData = res.info.data[0];
						self.price = self.cardData.price;
						var nowTime = Date.parse(new Date());
						if(self.userData.deadline!=0){
							console.log(self.userData.deadline)
							if(self.userData.deadline<nowTime/1000){
								self.userData.deadline = nowTime/1000;
							}
							self.mainData.deadline =self.userData.deadline+ parseInt(self.cardData.duration) *86400;
							console.log(self.mainData.deadline,nowTime/1000)
							// self.mainData.deadline =
							// 	self.userData.deadline != 0 ? self.userData.deadline+ parseInt(self.cardData.duration) *
							// 	86400 : Date.parse(new Date()) / 1000 + parseInt(self.cardData.duration) *
							// 	86400;
						}else{
							self.mainData.deadline =nowTime/1000 + parseInt(self.cardData.duration) *86400;
						}
						// self.mainData.shop_no = self.cardData.shop_no;
					}
					console.log(self.cardData,'carData')
				};
				self.$apis.productGet(postData, callback);
			},

			getUserData() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						self.mainData.name = self.userData.name;
						self.mainData.gender = self.userData.gender;
						self.mainData.birthday = self.userData.birthday;
						self.mainData.phone = self.userData.phone;
						self.mainData.deadline = self.userData.deadline;
						// self.mainData.shop_no = self.userData.shop_no;
					}
					self.getCardData();
					console.log(self.userData,'用户')
					console.log(self.mainData,'mainData')
				};
				self.$apis.userInfoGet(postData, callback);
			},

			successSubmit() {
				const self = this;
				if (self.mainData.name == '') {
					self.$Utils.showToast('请输入姓名', 'none');
				} else if (self.mainData.birthday == '') {
					self.$Utils.showToast('请输入年龄', 'none');
				} else if (self.mainData.phone == '') {
					self.$Utils.showToast('请输入电话', 'none');
				} else if(!self.isAgree) {
					self.$Utils.showToast('请确认同意服务协议', 'none');
				}else {
					var reg = /^1[3456789]\d{9}$/
					if (reg.test(self.mainData.phone)) {
						self.Utils.stopMultiClick(self.submit);
					} else {
						self.$Utils.showToast('电话号码格式错误', 'none');
					}
				}
				uni.setStorageSync('canClick',true);
			},

			submit() {
				const self = this;
				var orderList = []
				orderList.push({
					product_id: self.product_id,
					count: 1,
					type: 2,
					data:{
						shop_no: self.cardData.shop_no
					}
				});
				console.log('testnb');
				wx.requestSubscribeMessage({
					tmplIds: [
						'qAz8Vn3an8LzQ52VplqDpKsoUTCQHM7E9g3Cd4opzvo'
					],
					success(res) {
						console.log('res', res)
						self.addOrder(orderList)
					}
				});
			},

			addOrder(orderList) {
				const self = this;
				const postData = {};
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.tokenFuncName = 'getProjectToken';
				console.log('addOrder', postData);
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
				self.mainData.behavior = 1;
				console.log('self.price', self.price)
				postData.wxPay = {
					price: parseFloat(self.price)
				};

				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				// postData.payAfter = [];
				postData.payAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						data: self.mainData,
						searchItem: {
							user_no: uni.getStorageSync('user_info').user_no
						}
					},
					{
						tableName: 'User',
						FuncName: 'update',
						data: {
						  shop_no: self.cardData.shop_no
						},
						searchItem: {
							user_no: uni.getStorageSync('user_info').user_no
						}
					}
				];
				console.log('postData',postData)
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
										self.$Router.redirectTo({
											route: {
												path: '/pages/user/user'
											}
										})
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
								self.$Router.redirectTo({
									route: {
										path: '/pages/user/user'
									}
								})
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

<style scoped>
	.inforIcon {
		width: 26rpx;
		height: 30rpx;
	}

	.inforIcon1 {
		width: 32rpx;
		height: 30rpx;
	}

	.inforIcon2 {
		width: 32rpx;
		height: 28rpx;
	}

	input {
		width: 400rpx;
		text-align: right;
		font-size: 26rpx;
	}

	.btnAuto {
		margin-top: 200rpx;
	}
	
	.colorB{color: #63D1F8;}
	.xy{height: 1000rpx;margin-top: 15%;}
</style>
