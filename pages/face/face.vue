<template>
	<view>
		
		<view class="txt text-center">
			<view class="colorf pb-2">如需重新修改，请联系管理员删除</view>
			<view class="colorM" @click="Router.navigateTo({route:{path:'/pages/faceApply/faceApply'}})">申请删除</view>
		</view>
		
		<view class="photo p-r">
			<image src="../../static/images/face-recogontion-icon.png"></image>
			<view class="p-aXY flex0" v-if="submitData.mainImg.length>0">
				<image class="uploadImg" :src="submitData&&submitData.mainImg&&submitData.mainImg[0].url" @click="upLoadImg('mainImg')" mode="widthFix"></image>
			</view>
			<view v-else class="wh300 radius-5 Mgb p-aXY" @click="upLoadImg('mainImg')">
				<image src="../../static/images/face-recogontion-icon1.png" class="img"></image>
			</view>
			<!-- <view >照片已上传</view> -->
			<!-- <image v-else class="uploadImg p-aXY" :src="submitData.mainImg[0].url" @click="upLoadImg('mainImg')" mode="widthFix"></image> -->
		</view>
		
		<view class="colorf text-center btnAuto" @click="submit">
			确认提交
		</view>
		
		<!-- <form action='https://www.airenche.com/shop/UploadImage?tk=5381' method='post' id="documentForm">
		     <input type='file' name='imageFile' id="imageFile"/>
			 <input type="submit" value="提交">
		</form> -->
		
		<!-- <view class="bg-mask" v-show="is_show">
			<view class="bg-white p-aX bottom-0 text-center">
				<view class="py-3">拍照</view>
				<view class="py-3 bT-e1">从相册中选择</view>
				<view class="f5Bj-H20"></view>
				<view class="py-3 bT-e1" @click="isShow">取消</view>
			</view>
		</view> -->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				submitData:{
					photo:'',
					mainImg:[]
				}
			}
		},
		onLoad(){
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
		},
		onShow() {
			const self = this;
			self.getUserData();
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getProjectToken';
				
				const callback = (res) => {
					if (res.info.data) {
						self.submitData.photo = res.info.data[0].photo;
						self.submitData.mainImg = res.info.data[0].mainImg;
						console.log(self.submitData,'信息')
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit(){
				const self = this;
				const postData = {
					data:self.submitData
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					console.log('正在提交')
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '提交成功',
						    duration: 2000,
						});
						setTimeout(function(){
							uni.navigateBack({
							    delta: 1
							});
						},2000);
					} else {
						self.$Utils.showToast('网络故障', 'none')
						console.log(222222)
					};
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res) {
						self.submitData.photo = res;
						console.log(res,'硬件服务器上传成功')
						console.log(self.submitData,'submitData')
						self.$Utils.showToast('上传成功，请确认提交', 'none')
					}else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};
				const callbackTwo = (res) => {
					console.log('res', res)
					if (res) {
						self.submitData.mainImg = [{'url':res.info.url,'type':'image'}];
						console.log(res.info,'本地服务器上传成功')
						self.$Utils.showToast('数据更新成功', 'none')
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
						
						// 上传本地服务器
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'mainImg'
						}, callbackTwo)
						
						// 上传硬件服务器
						wx.uploadFile({
							url: 'https://www.airenche.com/shop/UploadImage?tk=5381',
							filePath: tempFilePaths,
							name: 'pic',
							formData: {},	
							success: function(res) {
								console.log('callbackp',res)
								if (res.data) {
									res.data = res.data.match(/"filename":"(\S*)"},"msg"/)[1];
									self.submitData.photo = res.data;
								};
								if (res.data.solely_code == '200000') {
									token[formData.tokenFuncName](c_callback, {
										refreshToken: true
									});
								} else {
									callback && callback(res.data);
								};
							},
							fail: function(err) {
								wx.showToast({
									title: '网络故障',
									icon: 'fail',
									duration: 2000,
									mask: true,
								});
							}
						})
						
						
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
page{background-color: #000000;}
</style>
<style scoped>
.txt{margin-top: 110rpx;}
.photo{width: 440rpx;height: 440rpx;margin: 130rpx auto 0;}
.img{width: 109rpx;height: 92rpx;position: absolute;margin: auto;left: 0;top: 0;right: 0;bottom: 0;}
.wh300{margin: 70rpx;}
.uploadImg{width: 100%;max-height: 600rpx;}
.btnAuto{width: 600rpx;margin-top: 200rpx;}
</style>