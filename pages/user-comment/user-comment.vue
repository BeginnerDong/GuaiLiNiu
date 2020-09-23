<template>
	<view>
		
		<view class="shadowM px-2 mx-2 mt-3 bg-white radius10 line-h">
			<view class="font-24 color6 flex1 py-3 bB-f5">
				<view>订单编号：{{mainData.order_no}}</view>
				<view class="colorR">待评价</view>
			</view>
			<view class="flex1 py-3 bB-f5 w-100">
				<image :src="mainData.product[0].mainImg[0].url" class="wh180 radius10"></image>
				<view class="px-2 flex-1 d-flex flex-column j-sb h-180">
					<view class="font-30 font-w">{{mainData.product[0].title}}</view>
					
					<!-- 团课评价显示 -->
					<view v-show="type==0"><text class="font-w price">{{mainData.product[0].price}}</text>/课时</view>
					<view class="font-24 line-h-sm" v-show="type==0"><!-- Auger | {{mainData.product[0].book_week_item}}~ -->{{mainData.product[0].book_time_item}}</view>
					
					<view class="flex">
						<block v-for="(c_item,c_index) in mainData.product[0].description_change" :key="c_index">
							<view class="tag">{{c_item}}</view>
						</block>
					</view>
					
					<!-- 私教课评价显示 -->
					<view class="colorR" v-show="type==1"><text class="price">{{mainData.product[0].price}}</text>/课时</view>
				</view>
			</view>
			<!-- <view class="font-26 color6 py-3 bB-f5">课程有效期：{{mainData.product[0].duration}}天 </view> -->
		</view>
		
		<view class="shadowM bg-white mx-2 mt-2 px-2 py-3 radius10">
			<textarea v-model="data.description" value="" placeholder="请填写评论,让更多人知道" class="comment"/>
			<view class="flex font-22 color9 flex-wrap">
				<block v-for="(item,index) in  data.bannerImg" :key="index">
					<image :src="item.url" class="wh100 mr-2"></image>
				</block>
				
				
				<image src="../../static/images/evaluation-img.png" class="wh100 mr-2" @click="upLoadImg('bannerImg')"></image>
				<view>可上传多张图片</view>
			</view>
		</view>
		
		<view class="btnAuto" @click="Utils.stopMultiClick(submit)">确定</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				type:0,
				mainData:{},
				data:{
					title:'',
					mainImg:[],/* 头像*/
					bannerImg:[],
					description:'',
					relation_table:'product',
					relation_id:'',
					passage1:'',/* 门店NO*/
					passage2:''/* 教练NO*/,
					thirdapp_id:2
				},
				Utils:this.$Utils
			}
		},
		onLoad(option){
			const self = this;
			var userInfo = uni.getStorageSync('user_info');
			self.mainData = uni.getStorageSync('orderDetail');
			self.data.title = userInfo.nickname;
			self.data.user_no = userInfo.user_no;
			self.data.user_type = userInfo.user_type;
			if(userInfo.headImgUrl){
				self.data.mainImg = [{url:userInfo.headImgUrl}];
			};
			console.log('course',self.mainData)
			self.data.relation_id = self.mainData.product.id;
			self.data.passage1 = self.mainData.shop_no;
			self.data.passage2 = self.mainData.coach_no;
			
			self.type = option.type;
			uni.setStorageSync('canClick', true);
		},
		methods: {
			submit(){
				const self = this;
				const postData = {
					data:self.data
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.saveAfter = [
					{
						tableName: 'Order',
						FuncName: 'update',
						data: {
							isremark:1
						},
						searchItem:{
							id:self.mainData.id
						}
					}
				]
				const callback = (res) => {
					
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '评论成功',
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
			
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.data[type].push({url:res.info.url,type:'image'})
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					sourceType:['camera'],
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.h-180{height: 180rpx;}
.comment{width: 710rpx;height: 275rpx;color: #999;font-size: 24rpx;}
.btnAuto{width: 710rpx;margin-top: 20rpx;}
</style>
