<template>
	<view>

		<image src="../../static/images/the loginl-img.png" class="login-logo"></image>

		<view class="flex m-75 bB-f5 pb-3 mb-5">
			<image src="../../static/images/the loginl-icon.png" class="zh-icon"></image>
			<input v-model="mainData.login_name" type="text" value="" placeholder="请输入账号" />
		</view>
		<view class="flex m-75 bB-f5 pb-3 mb-5">
			<image src="../../static/images/the loginl-icon1.png" class="zh-icon"></image>
			<input v-model="mainData.password" type="text" value="" placeholder="请输入密码" />
		</view>

		<view class="btnAuto mt-1" @click="Utils.stopMultiClick(submit)">确定</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				mainData: {
					login_name: '',
					password: ''
				},
				Utils: this.$Utils
			}
		},
		onLoad() {
			uni.setStorageSync('canClick', true);
		},
		methods: {
			submit() {
				const self = this;

				uni.setStorageSync('login', self.mainData);
				var callback = function(res) {
					// console.log(res);
					uni.setStorageSync('canClick', true);
					if (res.data.solely_code == 100000) {
						console.log('登录成功')
						self.$Router.redirectTo({
							route: {
								path: '/pages/user-sijiaoEntrance/user-sijiaoEntrance'
							}
						})
					} else {
						uni.showModal({
							title: '登陆失败',
							content: res.msg,
							showCancel: false
						})
					};


				};
				self.$Token.getCoachToken(callback);
			}
		}
	}
</script>

<style scoped>
	.login-logo {
		width: 200rpx;
		height: 207rpx;
		margin: 100rpx auto;
	}

	.zh-icon {
		width: 30rpx;
		height: 36rpx;
		margin-right: 20rpx;
	}

	input {
		font-size: 26rpx;
		color: #999;
	}

	.m-75 {
		margin-left: 75rpx;
		margin-right: 75rpx;
	}

	.btnAuto {
		width: 600rpx;
	}
</style>
