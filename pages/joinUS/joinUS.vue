<template>
	<view>
		
		<view class="Mgb px-2 pt-3 colorf text-center line-h">
			<image src="../../static/images/to join in-img.png" class="logo"></image>
			<view class="font-60 pt-5">健身就选怪力牛运动</view>
			<view class="font-36 pt-3 pb-5">提供最好的服务、最好就业条件</view>
			<view class="flex0">
				<view class="text" v-for="(v,index) in text" :key="index">{{v}}</view>
			</view>
			<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="img1"></image>
		</view>
		
		<view class="ru px-2 pb-5">
			<view class="font-36 line-h pt-5 pb-2 text-center p-r mb-4 tit">申请入口</view>
			<input type="text" v-model="submitData.title" placeholder="请输入您的姓名" />
			<input type="text" v-model="submitData.phone" placeholder="请输入您的手机号" />
			<input type="text" v-model="submitData.description" placeholder="请输入您选择的区域" />
			<view class="btnAuto" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">确定</view>
		</view>
		
		<view class="px-2">
			<view class="font-36 line-h pt-5 pb-2 text-center p-r mb-4 tit">加盟条件</view>
			<view   class="content ql-editor" style="padding:0;" v-html="mainData.content">
				
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				text:['专','业','一','对','一','服','务'],
				mainData:{},
				submitData:{
					title:'',
					phone:'',
					description:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('留言成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			
			
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
					const callback = (user, res) => {
						console.log(res)
						self.messageAdd();
					};
					self.$Utils.getAuthSetting(callback);
				
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入留言内容', 'none')
				};
			},
			
			getMainData() {
				var self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					menu_id:4
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.logo{width: 196rpx;height: 52rpx;}
.text{width: 50rpx;line-height: 50rpx;border-radius: 50%;color: #222;font-size: 30rpx;text-align: center;background-color: #FFCDC0;margin: 0 15rpx;}
.img1{width: 413rpx;height: 463rpx;margin: auto;}
.ru{background-color: #FFF3EB;}
.tit::before{content: '';background-color: #FF633A;width: 60rpx;height: 4rpx;position: absolute;bottom: 0;left: 50%;margin-left: -25rpx;}
input{width: 710rpx;height: 80rpx;font-size: 26rpx;border: 1px solid #e1e1e1;background-color: #fff;border-radius: 10rpx;margin-bottom: 30rpx;text-indent: 20rpx;}

.joinIcon{width: 38rpx;height: 34rpx;}

.xh{color: #fff;position: absolute;top: 0;left: 0;line-height: 34rpx;width: 38rpx;text-align: center;}
.img2{width: 500rpx;height: 300rpx;}
.img2:nth-child(2){margin-top: -80rpx;float: right;}
</style>
