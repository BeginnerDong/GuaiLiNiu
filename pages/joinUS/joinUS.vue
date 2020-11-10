<template>
	<view>
		
		<view class="Mgb px-2 pt-3 colorf text-center line-h">
			<image src="../../static/images/to join in-img.png" class="logo"></image>
			<view class="font-60 pt-5">健身就选怪力牛运动</view>
			<view class="font-36 pt-3 pb-5">提供最好的服务、最好就业条件</view>
			<view class="flex0">
				<view class="text" v-for="(v,index) in text" :key="index">{{v}}</view>
			</view>
			<view class="py-4 img1">
				<video :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" class="w-100 h-100"></video>
			</view>
		</view>
		
		<view class="ru px-2 pb-5">
			<view class="font-36 line-h pt-5 pb-2 text-center p-r mb-4 tit">申请入口</view>
			<input type="text" v-model="data.title" placeholder="请输入您的姓名" />
			<input type="text" v-model="data.phone" placeholder="请输入您的手机号" />
			<input type="text" v-model="data.description" placeholder="请输入您选择的区域" />
			<view class="btnAuto" @click="successSubmit">确定</view>
		</view>
		
		<view class="">
			<view class="px-2 font-36 line-h pt-5 pb-2 text-center p-r mb-4 tit">加盟条件</view>
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
				data:{
					title:'',
					phone:'',
					description:'',
					type:1
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			successSubmit(){
				const self = this;
				if(self.data.title == ''){
					self.$Utils.showToast('请输入姓名','none')
				}else if(self.data.phone == ''){
					self.$Utils.showToast('请输入电话号码','none')
				}else if(self.data.description == ''){
					self.$Utils.showToast('请输入您的区域','none')
				}else{
					var reg = /^1[3456789]\d{9}$/
					if(reg.test(self.data.phone)){
						self.submit()
					}else{
						self.$Utils.showToast('电话号码格式错误','none')
					}
				}
			},
			
			
			submit(){
				const self = this;
				const postData = {
					data:self.data
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '已提交申请',
						    duration: 2000,
						});
						setTimeout(function(){
							uni.navigateBack({
							    delta: 1
							});
						},2000)
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};
				self.$apis.messageAdd(postData, callback);
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
.img1{width: 100%;height: 400rpx;margin: auto;}
.ru{background-color: #FFF3EB;}
.tit::before{content: '';background-color: #FF633A;width: 60rpx;height: 4rpx;position: absolute;bottom: 0;left: 50%;margin-left: -25rpx;}
input{width: 710rpx;height: 80rpx;font-size: 26rpx;border: 1px solid #e1e1e1;background-color: #fff;border-radius: 10rpx;margin-bottom: 30rpx;padding: 0 20rpx;box-sizing: border-box;}

.joinIcon{width: 38rpx;height: 34rpx;}

.xh{color: #fff;position: absolute;top: 0;left: 0;line-height: 34rpx;width: 38rpx;text-align: center;}
.img2{width: 500rpx;height: 300rpx;}
.img2:nth-child(2){margin-top: -80rpx;float: right;}
</style>
