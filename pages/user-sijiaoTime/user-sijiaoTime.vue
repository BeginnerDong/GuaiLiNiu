<template>
	<view class="h-100 flexY">
		
		<view class="flex1 py-3 px-2 bB-e1 w-100">
			<image :src="mainData.product[0].mainImg[0].url" class="wh120"></image>
			<view class="px-2 flex5 flex-1 h-120">
				<view class="font-30 font-w flex-1">{{mainData.product[0].title}}</view>
				<view class="flex">
					<block v-for="(c_item,c_index) in mainData.product[0].description_change" :key="c_index">
						<view class="tag">{{c_item}}</view>
					</block>
				</view>
			</view>
		</view>
		
		<view class="flex-1 d-flex">
			<view class="left bR-e1 h-100 bg-f5">
				<block v-for="(item,index) in timeList" :key="index">
					<view class="py-3 bB-e1" :class="leftCurr==index?'on':''" @click="changeLeft(index)">
						<view>{{item.m+'.'+item.d}}</view>
						<view class="font-20 color9 date">{{week[item.ds]}}</view>
					</view>
				</block>
			</view>
			
			<view class="flex-1 h-100 flexY right">
				
				<view class="d-flex flex-wrap">
					<block v-for="(item,index) in canChooseHour" :key="index">
						<view class="jl color6 shadowM p-r mt-5" @click="chooseHour(item)" >
							<view>{{item}}</view>
							<image src="../../static/images/yuyue-icon-01.png" class="wh26" v-if="choosedHour == item"></image>
							<image src="../../static/images/yuyue-icon-02.png" class="wh26" v-else></image>
						</view>
					</block>
					
				</view>
				
			</view>
		</view>
		
		<view class="bT-e1 p-2 bg-white">
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
				},
				timeList:[],
				week:['周日','周一','周二','周三','周四','周五','周六'],
				canChooseWeek:[],
				canChooseHour:[],
				choosedHour:'',
				mainData:{}
			}
		},
		onLoad(option){
			const self = this;
			var userInfo = uni.getStorageSync('user_info');
			self.mainData = uni.getStorageSync('orderDetail');
			self.data.order_no = self.mainData.order_no;
			self.data.product_id = self.mainData.product_id;
			self.data.course_type = self.mainData.course_type;
			self.data.shop_no = self.mainData.shop_no;
			self.data.coach_no = self.mainData.product[0].coach_no;
			
			self.timeList = self.$Utils.getFutureDateList(13);
			self.canChooseWeek = self.mainData.product[0].book_week_item.split(',');
			self.canChooseHour = self.mainData.product[0].book_time_item.split(',');
			uni.setStorageSync('canClick', true);
		},
		methods: {
			
			chooseHour(item){
				const self = this;
				self.choosedHour = item;
			},
			changeLeft(i){
				const self = this;
				self.leftCurr = i;
			},
			submit(){
				const self = this;
				/* if(self.mainData.orderLog.length>=parseInt(self.mainData.standard)){
					self.$Utils.showToast('预约次数已经完成', 'none')
					return;
				}; */
				self.data.book_time = 
				self.timeList[self.leftCurr].y
				+'-'+self.timeList[self.leftCurr].m
				+'-'+self.timeList[self.leftCurr].d
				+'  '+self.choosedHour;
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
					console.log(res)
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
						uni.showToast({
						    title: res.mgs,
						    duration: 2000,
						});
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

.left{width: 140rpx;text-align: center;overflow-y: auto;}
.left .on{background-color: #fff;color: #FF633A;}
.left .on .date{color: #FF633A;}
.right{padding: 30rpx 30rpx 80rpx;overflow-y: auto;}
.jl{margin-right: 68rpx;width: 140rpx;line-height: 80rpx;text-align: center;}
.jl:nth-child(3n){margin-right: 0;}

.wh26{position: absolute;top: -10rpx;right: -10rpx;}
</style>
