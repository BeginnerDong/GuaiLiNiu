<template>
	<view class="font-30">
		
		<view class="px-2 bg-white">
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">上传照片</view>
				<view v-if="">
					<image src="../../static/images/img3.png" class="scImg"
					@click="upLoadImg('mainImg')" v-if="submitData.mainImg.length<=0"></image>
					<image :src="submitData.mainImg[0].url" class="scImg" v-else></image>
				</view>
				<!-- <image src="../../static/images/Vpreferential-img.png" class="scImg"></image> -->
			</view>
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">姓名</view>
				<input type="text" v-model="mainData.name" placeholder="请输入" />
			</view>
			<view class="flex1 py-4 bB-f5">
				<view class="flex-1">年龄</view>
				<input type="text" v-model="mainData.birthday" placeholder="请输入" />
			</view>
			<view class="flex1 py-4 bB-f5">
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
			<view class="btnAuto" @click="successSubmit">确定</view>
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
					id_free:1,
					mainImg:[]
				},
				submitData:{
					mainImg:[]
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
			// console.log('(new Date()).toLocaleDateString()',(new Date()).toLocaleDateString());
			// console.log('self.mainData',self.mainData);
			console.log('submitData',self.submitData.mianImg)
			self.getUserData();
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
					}
					console.log(res)
					console.log(self.userData)
					console.log(self.mainData)
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			successSubmit() {
				const self = this;
				if(self.submitData.mainImg.length <= 0){
					self.$Utils.showToast('请上传图片', 'none')
				}else if (self.mainData.name == '') {
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
			
			// submit(){
			// 	const self = this;
			// 	const postData = {
			// 		data:self.submitData
			// 	};
			// 	postData.tokenFuncName = 'getProjectToken';
			// 	const callback = (res) => {
			// 		uni.setStorageSync('canClick', true);
			// 		if (res.solely_code == 100000) {
			// 			uni.showToast({
			// 			    title: '提交成功',
			// 			    duration: 2000,
			// 			});
			// 			setTimeout(function(){
			// 				uni.navigateTo({
			// 				    path: '/pages/experienceUser/experienceUser'
			// 				});
			// 			},2000);
			// 		} else {
			// 			self.$Utils.showToast('网络故障', 'none')
			// 		};
			// 	};
			// 	self.$apis.userUpdate(postData, callback);
			// },
			
			submit(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.mainData.mainImg[0] = self.searchItem.mainImg[0];
				console.log('submit',self.submitData,'main',self.mainData)
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
				self.$apis.userUpdate(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;	
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData['mainImg'].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
					}else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						console.log('img',self.submitData)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
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
.scImg{width: 210rpx;height: 130rpx;border-radius: 20rpx;}

.btnAuto{margin-top: 200rpx;}
</style>