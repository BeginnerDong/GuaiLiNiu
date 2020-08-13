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
		<!-- <button class="carBtn" open-type="getUserInfo" @getuserinfo="submit" >立即预约</button> -->
		<button class="btnAuto" @click="successSubmit">确认</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				product_id: 0,
				mainData: {
					name: '',
					gender: 0,
					birthday: '',
					phone: '',
					deadline: 0,
					behavior: 1
				}
			}
		},
		onLoad(options) {
			const self = this;
			self.product_id = options.product_id;
			self.price = options.price;
			self.duration = options.duration
			console.log('options', options)
			self.getUserData()

			console.log('(new Date()).toLocaleDateString()', (new Date()).toLocaleDateString());
			console.log('self.mainData', self.mainData);
		},
		methods: {

			getUserData() {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						self.mainData.name = self.userData.name
						self.mainData.gender = self.userData.gender
						self.mainData.birthday = self.userData.birthday
						self.mainData.phone = self.userData.phone
						self.mainData.deadline =
							self.userData.deadline != 0 ? self.userData.deadline : Date.parse(new Date()) / 1000 + parseInt(self.duration) *
							86400;

					}
					console.log(self.mainData)
				};
				self.$apis.userInfoGet(postData, callback);
			},

			successSubmit() {
				const self = this;
				if (self.mainData.name == '') {
					self.$Utils.showToast('请输入姓名', 'none')
				} else if (self.mainData.birthday == '') {
					self.$Utils.showToast('请输入年龄', 'none')
				} else if (self.mainData.phone == '') {
					self.$Utils.showToast('请输入电话', 'none')
				} else {
					var reg = /^1[3456789]\d{9}$/
					if (reg.test(self.mainData.phone)) {
						self.submit()
					} else {
						self.$Utils.showToast('电话号码格式错误', 'none')
					}
				}
			},

			submit() {
				const self = this;

				var orderList = []
				orderList.push({
					product_id: self.product_id,
					count: 1,
					type: 1
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
				console.log('self.price', self.price)
				postData.wxPay = {
					price: parseFloat(self.price)
				};

				postData.tokenFuncName = 'getProjectToken',
					postData.searchItem = {
						id: self.orderId
					};
				postData.payAfter = [];
				postData.payAfter.push({
					tableName: 'UserInfo',
					FuncName: 'update',
					data: self.mainData,
					searchItem: {
						user_no: uni.getStorageSync('user_info').user_no
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
</style>
