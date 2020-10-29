<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">全部</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">待使用</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">进行中</view>
				<view class="li" :class="navCurr==3?'on':''" @click="changeNav(3)">待评价</view>
				<view class="li" :class="navCurr==4?'on':''" @click="changeNav(4)">已评价</view>
			</view>
		</view>
		<view class="f5Bj-H20 mb-3"></view>
		<block v-for="(item,index) in mainData" :key="index">
			<view class="shadowM px-2 mx-2 mb-3 radius10 line-h">
				<view class="font-24 color6 flex1 py-3 bB-f5">
					<view>订单编号：{{item.order_no}}</view>
					<view class="colorR" v-show="item.transport_status==0">待使用</view>
					<view class="colorR" v-show="item.transport_status==1">预约中</view>
					<view class="colorR" v-show="item.transport_status==2&&item.isremark==0">待评价</view>
					<view class="colorR" v-show="item.transport_status==3&&item.isremark==1">已评价</view>
				</view>
				<view class="flex1 py-3 bB-f5 w-100">
					<image :src="item.product[0].mainImg&&item.product[0].mainImg[0]?item.product[0].mainImg[0].url:''" class="wh180 radius10"></image>
					<view class="px-2 flex-1 flex-1 w490">
						<view class="font-30 font-w">{{item.product[0].title}}
							<text class="colorR font-28 pl-2" v-show="item.course_type==2">(共{{item.count}}节)</text>
						</view>
						<view class="pt-2"><text class="font-w price">{{item.product[0].price}}</text>/课时</view>
						<view class="font-24 py-1 line-h-sm avoidOverflow">{{item.coach.name?item.coach.name:''}} 
							<text class="ml-1" v-show="item.product[0].course_type==2">| {{changeTime}}~{{item.product[0].book_time_item}}</text>
						</view>
						<view class="flex">
							<block v-for="(c_item,c_index) in item.product[0].description_change" :key="c_index">
								<view class="tag" v-if="c_item">{{c_item}}</view>
							</block>
							
						</view>
					</view>
				</view>
				<!-- <view class="font-26 color6 py-3 bB-f5">课程有效期：{{item.product[0].duration}}天 </view> -->
				<view class="font-26 color6 py-3 bB-f5 flex1" v-show="item.transport_status==0&&item.course_type==2">
					<view>已使用{{item.orderLog.length}}节，剩余<text class="colorR">{{item.count-item.orderLog.length}}</text>节</view>
				</view>
				<!-- 其他 -->
				<view class="py-3 d-flex j-end" v-show="item.transport_status!=1">
					<view class="btn b-e1" v-show="item.transport_status==0" @click="goNext('use',item)">立即使用</view>
					<view class="btn b-e1" v-show="item.transport_status==2&&item.isremark==0" @click="goNext('comment',item)">立即评价</view>
					<view class="btn b-e1" v-show="item.transport_status==3&&item.isremark==1" @click="goNext('checkComment',item)">查看评价</view>
				</view>
				<view>
					<block v-for="(cc_item,index) in item.orderLog" :key="index">
						<view class="flex1 py-3 bT-f5">
							<span>预约时间：{{cc_item.book_time_change}} {{cc_item.is_book==1?'已成团':'未成团'}}</span>
							<img :src="cc_item.qrcode" style="width: 40px;height: 40px;"
							@click="bigImg(cc_item.qrcode)" v-show="cc_item.qrcode"></img>
						</view>
					</block>
					
				</view>
				
				<!-- 进行中 -->
				<!-- <view class="py-3 flex1">
					<view>核销码：</view>
					<image src="../../static/images/my class-img.png" class="wh80"></image>
				</view> -->
			</view>
		</block>
		
		
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
					thirdapp_id:2,
					course_type:['in',[1,2]],
					pay_status:1
				},
				isLoadAll:false,
				week:['周日','周一','周二','周三','周四','周五','周六'],
				changeTime:[],
				data:{
					order_no:'',
					product_id:'',
					course_type:'',
					book_time:'',
					shop_no:''
				},
				paginate:{}
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem = {
				thirdapp_id:2,
				course_type:['in',[1,2]]
			};
			self.getMainData();
			// self.$Utils.loadAll(['getMainData'], self);
		},
		// onShow(){
			// const self = this;
			// if(self.mainData)
			// self.getMainData(true);
		// },
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			weekTime(time){
				const self = this;
				self.changeTime = [];
				for(var i=0; i<time.length; i++){
					if(self.week[time[i]]){
						self.changeTime.push(self.week[time[i]]);
					}
				}
				self.changeTime = self.changeTime.toString();
			},
			
			bigImg(url){
				let _this = this;
				let imgsArray = [];
				imgsArray[0] = url
				uni.previewImage({
					current: 0,
					urls: imgsArray
				});
			},
				
			goNext(type,item){
				const self = this;
				uni.setStorageSync('orderDetail',item);
				if(type=="comment"){
					self.Router.navigateTo({route:{path:'/pages/user-comment/user-comment?type=0'}});
				}else if(type=="use"){
					console.log(item.id)
					if(item.course_type==1){
						uni.setStorageSync('Canclick',false);
						self.data.order_no = item.order_no;
						self.data.product_id = item.product[0].id;
						self.data.course_type = item.course_type;
						self.data.shop_no = item.shop_no;
						self.data.coach_no = item.shop_no;
						
						self.data.book_time = self.$Utils.timeto(parseInt(item.product[0].start_time),'ymd-hm');
						console.log(self.data)
						self.successSubmit(item.id);
					}else{
						self.Router.navigateTo({route:{path:'/pages/user-sijiaoTime/user-sijiaoTime?type='+item.course_type+'&id='+item.id}});
					}
				}else if(type=="checkComment"){
					self.Router.navigateTo({route:{path:'/pages/user-commentDetail/user-commentDetail?type=0'}})
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'product_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					orderLog:{
						tableName:'OrderLog',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'=',
					},
					coach:{
						tableName:'Coach',
						middleKey:'coach_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].product[0].description_change= self.mainData[i].product[0].description.split(',');
							if(self.mainData[i].product[0].course_type==2){
								self.weekTime(self.mainData[i].product[0].book_week_item.split(','));
							}
							console.log('周数',self.mainData[i].product[0].book_week_item)
							for(var j=0;j<self.mainData[i].orderLog.length;j++){
								self.mainData[i].orderLog[j]['book_time_change'] = 
								self.$Utils.timeto(parseInt(self.mainData[i].orderLog[j]['book_time'])*1000,'ymd-hm');
							}
						};
					}else{
						self.isLoadAll = true;					
					};
					// console.log('main',self.mainData)
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			changeNav(i){
				const self = this;
				self.navCurr = i;
				
				
				switch(i) {
					case 0:
						self.searchItem = {
							thirdapp_id:2,
							course_type:['in',[1,2]],
							pay_status:1
						};
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
						break;
					case 1:
						self.searchItem = {
							thirdapp_id:2,
							course_type:['in',[1,2]],
							transport_status:['in',[0,1]],
							pay_status:1
						};
						break;
					case 2:
						self.searchItem = {
							thirdapp_id:2,
							course_type:['in',[1,2]],
							transport_status:1,
							pay_status:1
						};
						break;
					case 3:
						self.searchItem = {
							thirdapp_id:2,
							course_type:['in',[1,2]],
							transport_status:2,
							isremark:0,
							pay_status:1
						};
						break;
					case 4:
						self.searchItem = {
							thirdapp_id:2,
							course_type:['in',[1,2]],
							transport_status:2,
							isremark:1,
							pay_status:1
						};
						break;
				};
				
				self.getMainData(true);
				
			},
			
			successSubmit(id){
				const self = this;
				wx.requestSubscribeMessage({
					tmplIds: [
						'BZ74KvhYwLWYKzcY-OunKaWIkbsBy_wWZ01LaZsGlKo',
						'Z5oqs6UaEnLygi7S7XSZ_dQbU71cauG0peB9qrwqrF8'
					],
					success(res) {
						// console.log('res', res)
						self.submit(id)
					}
				});
			},
			
			submit(id){
				const self = this;
				console.log('时间',self.data.book_time)
				const postData = {
					data:self.data
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.saveAfter = [
					{
						tableName: 'Order',
						FuncName: 'update',
						searchItem:{
							id:id
						},
						data:{
							transport_status:1
						}
					}
				];
				const callback = (res) => {
					console.log(res)
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '使用成功',
						    duration: 2000,
						});
						self.changeNav(1);
					} else {
						uni.showToast({
						    title: res.msg,
						    duration: 2000,
						});
					}
				};
				self.$apis.orderLogAdd(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.li{width: 20%;}
.btn{width: 160rpx;line-height: 60rpx;text-align: center;border-radius: 5rpx;margin-left: 20rpx;}
</style>
