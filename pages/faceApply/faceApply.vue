<template>
	<view>
		
		<view class="bg-white px-2">
			<view class="py-3">绑定手机号：{{userData.phone}}</view>
			<view class="py-3 bT-e1">门店：{{shopData.name}}</view>
			<view class="py-3 bT-e1">店长手机号：{{shopData.phone}}</view>
		</view>
		
		<view class="btnAuto applyBtn" @click="submit">提交申请</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shopData:{},
				userData:{},
				
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
			self.shopData = uni.getStorageSync('shopData')
		},
		methods: {
			
			getUserData() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getProjectToken';
				
				const callback = (res) => {
					if (res.info.data) {
						self.userData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit(){
				const self = this;
				if(!self.userData.photo){
					self.$Utils.showToast('未上传照片', 'none');
					return;
				}
				const postData = {
					data:{
						photo:'',
						mainImg:[]
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.saveAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						searchItem:{
							id:self.userData.id
						}
					}
				];
				const callback = (res) => {
					res = JSON.parse(res.slice(7));
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '申请删除成功',
						    duration: 2000,
						});
						setTimeout(function(){
							uni.navigateBack({
							    delta: 1
							});
						},2000);
					} else {
						self.$Utils.showToast('网络故障', 'none')
						console.log('3333333')
					};
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.applyBtn{width: 600rpx;margin-top: 70%;}
</style>
