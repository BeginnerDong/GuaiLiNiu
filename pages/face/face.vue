<template>
	<view>
		
		<view class="txt text-center">
			<view class="colorf pb-2">如需重新修改，请联系管理员删除</view>
			<view class="colorM" @click="Router.navigateTo({route:{path:'/pages/faceApply/faceApply'}})">申请删除</view>
		</view>
		
		<view class="photo p-r">
			<image src="../../static/images/face recogontion-icon.png"></image>
			<view class="wh300 radius-5 Mgb p-aXY" v-if="submitData.mainImg.length<=0" @click="upLoadImg('mainImg')">
				<image src="../../static/images/face recogontion-icon1.png" class="img"></image>
			</view>
			<image :src="submitData.mainImg[0].url" class="uploadImg p-aXY" mode="widthFix" v-else></image>
		</view>
		
		<view class="colorf text-center btnAuto" @click="submit">
			确认上传
		</view>
		
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-aX bottom-0 text-center">
				<view class="py-3">拍照</view>
				<view class="py-3 bT-e1">从相册中选择</view>
				<view class="f5Bj-H20"></view>
				<view class="py-3 bT-e1" @click="isShow">取消</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false,
				submitData:{
					mainImg:[]
				}
			}
		},
		onLoad(){
			const self = this;
			console.log('submitData',self.submitData)
		},
		methods: {
			
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			},
			
			submit(){
				const self = this;
				// self.submitData.title = self.title;
				const postData = {
					data:self.submitData
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					
					uni.setStorageSync('canClick', true);
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
					};
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
page{background-color: #000000;}
</style>
<style scoped>
.txt{margin-top: 110rpx;}
.photo{width: 440rpx;height: 440rpx;margin: 130rpx auto 0;}
.img{width: 109rpx;height: 92rpx;position: absolute;margin: auto;left: 0;top: 0;right: 0;bottom: 0;}
.wh300{margin: 70rpx;}
.uploadImg{margin: auto;}
.btnAuto{width: 600rpx;margin-top: 200rpx;}
</style>