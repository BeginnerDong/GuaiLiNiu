<template>
	<view>
		
		<view class="p-r z10 bg-white p-s mb-2 top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">全部</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">预约中</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">已成团</view>
			</view>
		</view>
		<block v-for="(item,index) in mainData" :key="index">
			<view class="bg-white px-2 mx-2 mb-2 radius10 line-h p-r">
				<view class="colorf sgin">{{bookStatus[item.is_book]}}</view>
				<view class="flex1 py-3 bB-f5 w-100">
					<image :src="item.product.mainImg[0].url" class="img radius10"></image>
					<view class="px-2 py-2 flex-1 d-flex flex-column j-sb h-180">
						<view class="font-30 font-w">{{item.product.title}}</view>
						<view class="flex">
							<block v-for="(cc_item,cc_index) in item.descriptionArray" :key="cc_index">
								<view class="tag tagY"  >{{cc_item}}</view>
							</block>
						</view>
						<view class="colorR"><text class="price">{{item.product.price}}</text>/{{item.product.score}}课时</view>
					</view>
				</view>
				<view class="py-3 bB-f5 flex1">
					<view>预约时间</view>
					<view>{{item.book_time_change}}</view>
				</view>
				<view class="py-3">
					<view>预约学员（{{item.OrderLog.length}}）</view>
					<view class="flex flex-wrap">
						<block v-for="(c_item,c_index) in item.OrderLog" :key="index">
							<image :src="c_item.User.headImgUrl" class="wh80 radius-5" ></image>
						</block>
						
					</view>
				</view>
			</view>
		</block>
		
		
		<image src="../../static/images/sijiao-yuyue-img2.png" class="wh100 p-f" @click="getScancode"></image>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				navCurr:0,
				mainData:[],
				searchItem:{
					course_type:['in',[1,2]]
				},
				isLoadAll:false,
				paginate:{},
				bookStatus:['预约中','已成团'],
				coach:{}
				
			}
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.coach = uni.getStorageSync('coach_info');
			console.log('登陆教练',self.coach);
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getScancode: function() {
				const self = this;
			    uni.scanCode({
			        onlyFromCamera: true,
			        success: function (res) {
			            console.log('条码类型：' + res.scanType);
			            console.log('条码内容：' + res.result);
						if(res.result){
							self.getOrderLog(res.result)
						}
			        }
			    });
			 
			},
			
			getOrderLog(id){
				const self = this;
				var postData = {
					searchItem:{
						id:id,
						user_type:0
					},
					tokenFuncName : 'getCoachToken'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						var c_postData = {
							searchItem:{
								id:id,
								user_type:0
							},
							tokenFuncName : 'getCoachToken',
							data:{
								is_use:1
							}
						};
						if(!res.info.data[0].sign_time){
							c_postData.data.sign_time = (new Date().getTime()/1000);
						}else if(!res.info.data[0].in_time){
							c_postData.data.in_time = (new Date().getTime()/1000);
						}else if(!res.info.data[0].out_time){
							c_postData.data.out_time = (new Date().getTime()/1000);
						};
						
						self.updateOrderLog(c_postData)
					};
				};
				self.$apis.orderLogGet(postData, callback);
			},
			updateOrderLog(postData){
				const self = this;
				const callback = (res)=>{
					if(postData.sign_time){
						var title = "签到成功";
					}else if(postData.in_time){
						var title = "进场成功";
					}else{
						var title = "离场成功";
					};
					if(res.solely_code=100000){
						uni.showToast({
						    title:title,
						    duration: 2000,
						});
					}else{
						uni.showToast({
						    title:msg,
						    duration: 2000,
						});
					};
				};
				self.$apis.orderLogUpdate(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				const postData = {};
				postData.tokenFuncName = 'getCoachToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = self.coach.user_no;
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'product_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['coach_no','score','id','mainImg','title','description','duration','price','score']
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].descriptionArray = self.mainData[i].product.description.split(",");
							self.mainData[i].book_time_change = self.$Utils.timeto(parseInt(self.mainData[i].book_time*1000),'ymd-hm');
						};
					}else{
						self.isLoadAll = true;
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.courseGet(postData, callback);
			},
			
			changeNav(i){
				const self = this;
				if(i!=self.navCurr){
					self.navCurr = i;
					delete self.searchItem.is_book;
					switch(i) {
					     case 1:
					        self.searchItem.is_book = 0;
					        break;
					     case 2:
					        self.searchItem.is_book = 1;
					        break;
					};
					self.getMainData(true);
					
				}
				
				
			}
		}
	}
</script>
<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.li{width: 33.33%;}
.img{width: 200rpx;height: 160rpx;}
.wh80{margin-right: 39rpx;margin-top: 20rpx;}
.wh80:nth-child(6n){margin-right: 0;}
.h-180{height: 180rpx;}
.sgin{line-height: 40rpx;background-color: #FF633A;position: absolute;border-top-left-radius: 20rpx;border-bottom-left-radius: 20rpx; padding: 0 15rpx;right: 0;top: 20rpx;}
.wh100{top: 70%;right: 0;}
</style>
