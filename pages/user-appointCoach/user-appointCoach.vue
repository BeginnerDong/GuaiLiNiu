<template>
	<view class="h-100 flexY">
		
		<view class="flex1 py-3 px-2 bB-e1 w-100">
			<image src="../../static/images/the order-img.png" class="wh120"></image>
			<view class="px-2 flex5 flex-1 h-120">
				<view class="font-30 font-w flex-1">减脂训练营·中上</view>
				<view class="flex">
					<view class="tag tagY">矫正</view>
					<view class="tag tagB">提升柔软度</view>
					<view class="tag tagG">改善身体线条</view>
				</view>
			</view>
		</view>
		
		<view class="flex-1 d-flex">
			<view class="left bR-e1 h-100 bg-f5">
				<view class="py-3 bB-e1" :class="leftCurr==0?'on':''" @click="changeLeft(0)">
					<view>今天</view>
					<view class="font-20 color9 date">周日</view>
				</view>
				<view class="py-3 bB-e1" :class="leftCurr==1?'on':''" @click="changeLeft(1)">
					<view>明天</view>
					<view class="font-20 color9 date">周日</view>
				</view>
			</view>
			
			<view class="flex-1 h-100 flexY px-3 pb-4 right">
				
				<view class="d-flex flex-wrap">
					<view class="flex4 pb-1 jl" v-for="v in 5">
						<view class="pt-3 pb-2">8:00</view>
						<view class="shadowM p-2 p-r">
							<image src="../../static/images/home-img3.png" class="appoinImg"></image>
							<view class="colorf p-a mx-2 name">杨晓珊</view>
							
							<!-- <image src="../../static/images/yuyue-icon-01.png" class="wh26"></image> -->
							<image src="../../static/images/yuyue-icon-02.png" class="wh26"></image>
						</view>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="bT-e1 p-2">
			<view class="btnAuto" @click="submit">确定</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				leftCurr:0,
				data:{
					order_no:'',
					product_id:'',
					course_type:'',
					book_time:'',
					shop_no:''
				}
			}
		},
		onLoad(option){
			const self = this;
			var userInfo = uni.getStorageSync('user_info');
			self.mainData = uni.getStorageSync('orderDetail');
			self.data.order_no = self.mainData.order_no;
			self.data.product_id = self.mainData.product.id;
			self.data.course_type = self.mainData.product.course_type;
			self.data.shop_no = self.mainData.product.shop_no;
			
			self.data.book_time = (new Date()).getTime()/1000;
			uni.setStorageSync('canClick', true);
		},
		methods: {
			changeLeft(i){
				const self = this;
				self.leftCurr = i;
			},
			submit(){
				const self = this;
				if(self.mainData.orderLog.length>=parseInt(self.mainData.standard)){
					self.$Utils.showToast('预约次数已经完成', 'none')
					return;
				};
				const postData = {
					data:self.data
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.saveAfter = [
					{
						tableName: 'Order',
						FuncName: 'update',
						searchItem:{
							id:self.mainData.id
						}
					}
				];
				if((self.mainData.orderLog.length+1)==self.mainData.standard){
					postData.saveAfter[0]['data'] =  {
						transport_status:2
					};
				}else{
					postData.saveAfter[0]['data'] =  {
						transport_status:1
					};
				}
				const callback = (res) => {
					
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '预约成功',
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
				self.$apis.orderLogAdd(postData, callback);
			},
		}
	}
</script>
<style>
page{height: 100%;}
</style>
<style scoped>
.h-120{height: 120rpx;}
.btnAuto{width: 710rpx;}

.left{width: 140rpx;text-align: center;}
.left .on{background-color: #fff;color: #FF633A;}
.left .on .date{color: #FF633A;}
.appoinImg{width: 120rpx;height: 140rpx;}
.right .name{left: 0;right: 0;bottom: 20rpx;line-height: 40rpx; background: linear-gradient(to bottom, rgba(0,0,0,0) 20%,rgba(0,0,0,0.5) 100%);text-align: center;}
.jl{margin-right: 38rpx;}
.jl:nth-child(3n){margin-right: 0;}

.wh26{position: absolute;top: 10rpx;right: 10rpx;}
</style>
