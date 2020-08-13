<template>
	<view class="font-30">
		
		<view class="px-2 bg-white">
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">上传照片</view>
				<image src="../../static/images/img3.png" class="scImg"></image>
			</view>
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">姓名</view>
				<input type="text" v-model="mainData.name" placeholder="请输入" />
			</view>
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">年龄</view>
				<input type="text" v-model="mainData.birthday" placeholder="请输入" />
			</view>
			<view class="flex1 py-4">
				<view class="flex-1">性别</view>
				<view class="font-26 flex">
					<view class="flex" @click="mainData.gender=0">
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
				<view class="flex-1">电话</view>
				<input type="text" v-model="mainData.phone" placeholder="请输入" />
			</view>
		</view>
		
		<view class="px-2">
			<view class="btnAuto" @click="submit">确定</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{
					name:'',
					gender:0,
					birthday:'',
					phone:'',
					deadline:0,
					behavior:1,
					id_free:1
				}
			}
		},
		onLoad(options){
			const self = this;
			self.product_id = options.product_id;
			self.price = options.price;
			console.log('options',options)
			self.mainData.deadline = Date.parse(new Date())/1000 + 7*86400;
			/* if(uni.getStorageSync('user_info').info.deadline>Date.parse(new Date())){
				self.mainData.deadline = uni.getStorageSync('user_info').info.deadline + 7*86400;
			}else{
				self.mainData.deadline = Date.parse(new Date()) + 7*86400;
			}; */
			
			console.log('(new Date()).toLocaleDateString()',(new Date()).toLocaleDateString());
			console.log('self.mainData',self.mainData);
		},
		methods: {
			submit(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = self.mainData;	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.removeStorageSync('user_token');
						self.$Router.redirectTo({route:{path:'/pages/user/user'}})
					}else{
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					}
				};
				self.$apis.userInfoUpdate(postData, callback);
			}
		}
	}
</script>
<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.inforIcon{width: 26rpx;height: 30rpx;}
.inforIcon1{width: 32rpx;height: 30rpx;}
.inforIcon2{width: 32rpx;height: 28rpx;}
input{width: 400rpx;text-align: right;font-size: 26rpx;}
.scImg{width: 210rpx;height: 130rpx;}

.btnAuto{margin-top: 200rpx;}
</style>