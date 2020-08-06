<template>
	<view class="h-100 flexY bT-e1">
		
		<view class="flex-1 d-flex">
			<view class="left bR-e1 h-100 bg-f5">
				<block v-for="(item,index) in timeList" :key="index">
					<view class="py-3 bB-e1" :class="leftCurr==index?'on':''" @click="changeLeft(index)">
						<view>{{item.m+'.'+item.d}}</view>
						<view class="font-20 color9 date">{{week[item.ds]}}</view>
					</view>
				</block>
				
				<!-- <view class="py-3 bB-e1" :class="leftCurr==1?'on':''" @click="changeLeft(1)">
					<view>明天</view>
					<view class="font-20 color9 date">周日</view>
				</view> -->
			</view>
			
			<view class="flex-1 h-100 flexY right">
				
				<view class="d-flex flex-wrap">
					<block v-for="(item,index) in mainData" :key="index">
						<view class="flex py-2" >
							<view class="time">{{item.bookhm}}</view>
							<image :src="item.OrderLog[0].User.headImgUrl" class="wh80 mx-2"></image>
							<view>
								<view>{{item.OrderLog[0].UserInfo.name}}</view>
								<view class="font-26">{{item.OrderLog[0].UserInfo.phone}}</view>
							</view>
						</view>
					</block>
					
				</view>
				
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				leftCurr:0,
				mainData:[],
				searchItem:{
					course_type:3
				},
				timeList:[],
				week:['周日','周一','周二','周三','周四','周五','周六'],
				
			}
		},
		onLoad(options) {
			const self = this;
			self.timeList = self.$Utils.getFutureDateList(13);
			self.searchItem.book_time = ['in',[self.timeList[0].stime/1000,self.timeList[0].stime/1000+86400]];
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
				};
				const postData = {};
				postData.tokenFuncName = 'getCoachToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				/* postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'product_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['coach_no','score','id','mainImg','book_end_time','book_start_time','title','description','duration','price','score']
					},
					orderLog:{
						tableName:'OrderLog',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'=',
					}
				}; */
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].bookhm = self.$Utils.timeto(parseInt(self.mainData[i].book_time),'hm')
						};
					}else{
						self.isLoadAll = true;
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.courseGet(postData, callback);
			},
			
			changeLeft(i){
				const self = this;
				self.leftCurr = i;
				self.searchItem.book_time = ['in',[self.timeList[0].stime/1000,self.timeList[0].stime/1000+86400]];
				self.getMainData(true);
				
			}
		}
	}
</script>
<style>
page{height: 100%;}
</style>
<style scoped>
.left{width: 140rpx;text-align: center;}
.left .on{background-color: #fff;color: #FF633A;position: relative;}
.left .on::before{content: '';background-color: #FF633A;width: 8rpx;height: 100%;border-top-right-radius: 8rpx;border-bottom-right-radius: 8rpx;position: absolute;top: 0;left: 0;}
.left .on .date{color: #FF633A;}
.right{padding: 30rpx 30rpx 80rpx;}
.time{width: 80rpx;text-align: center;}
</style>
