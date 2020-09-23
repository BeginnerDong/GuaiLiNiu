<template>
	<view class="h-100 flex5">
		
		<view class="flex1 py-3 px-2 bB-e1 w-100 bg-white top">
			<image :src="mainData&&mainData.product&&mainData.product[0]&&mainData.product[0].mainImg&&mainData.product[0].mainImg[0]&&mainData.product[0].mainImg[0].url" class="wh120 radius10"></image>
			<view class="px-2 flex5 flex-1 h-120">
				<view class="font-30 font-w flex-1">{{mainData.product[0].title}}</view>
				<view class="font-24">{{mainData&&mainData.coach&&mainData.coach[0]&&mainData.coach[0].name}}</view>
				<view class="flex">
					<block v-for="(c_item,c_index) in mainData.product[0].description" :key="c_index">
						<view class="tag">{{c_item}}</view>
					</block>
				</view>
			</view>
		</view>
		
		<view class="d-flex cnter flex-1">
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
						<view class="jl color6 shadowM p-r mt-5" >
							<view class="p-aXY zzz" v-show="isUse[index]==0"></view>
							<view @click="chooseHour(item)">
								<view>{{isUse[index]==1?item.split('-')[0]:'已被预约'}}</view>
								<view v-show="isUse[index]==1">
									<image src="../../static/images/yuyue-icon-01.png" class="wh26" v-if="choosedHour == item.split('-')[0]"></image>
									<image src="../../static/images/yuyue-icon-02.png" class="wh26" v-else></image>
								</view>
							</view>
						</view>
					</block>
					
				</view>
				
			</view>
		</view>
		
		<view class="bT-e1 p-2 bg-white bto">
			<button class="btnAuto" open-type="getUserInfo" @click="successSubmit">确定</button>
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
				mainData:{},
				searchItem: {
					thirdapp_id: 2,
					course_type: 3
				},
				searchItem1:{
					// course_type: 3
				},
				product:{},
				productTime:[],
				timeData:[],
				isUse:[]
			}
		},
		onLoad(option){
			const self = this;
			var userInfo = uni.getStorageSync('user_info');
			
			self.searchItem.id = option.id;
			self.getMainData();
		},
		methods: {
			
			chooseHour(item){
				const self = this;
				self.choosedHour = item.split('-')[0];
			},
			
			changeLeft(i){
				const self = this;
				self.leftCurr = i;
				self.timeData = [];
				self.isUse = [];
				console.log('传入数据',i)
				
				for(var i=0; i<self.canChooseHour.length; i++){
					var date1 = '';
					date1 = self.timeList[self.leftCurr].y
				        +'-'+self.timeList[self.leftCurr].m
				        +'-'+self.timeList[self.leftCurr].d
				        +' '+self.canChooseHour[i].split('-')[0];
					//日期格式转时间戳
					var time1 = new Date(date1);
					self.timeData.push(Date.parse(time1)/1000)
				}
				// console.log('重复时间',self.timeData,self.productTime)
				
				for(var i=0; i<self.timeData.length; i++){
					self.isUse.push(1);
					for(var j=0; j<self.productTime.length; j++){
						if(self.productTime[j] == self.timeData[i]){
							self.isUse[i] = 0;         //0:已预约    1:未预约
						}
					}
				}
				console.log('可以使用的时间',self.isUse)
			},
			
			getProductData(){
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem1);
				const callback = (res) => {
					self.product = res.info.data[0];
					for(var i=0; i<self.product.course.length; i++){
						self.productTime.push(self.product.course[i].book_time);
					}
					self.changeLeft(0);
					// console.log('product',self.product)
				}
				self.$apis.productGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					product: {
						tableName: 'Product',
						middleKey: 'product_id',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '=',
					},
					orderLog: {
						tableName: 'OrderLog',
						middleKey: 'order_no',
						key: 'order_no',
						searchItem: {
							status: 1
						},
						condition: '=',
					},
					coach: {
						tableName: 'Coach',
						middleKey: 'coach_no',
						key: 'user_no',
						searchItem: {
							status: 1
						},
						condition: '='
					},
					course: {
						tableName: 'Course',
						middleKey: 'product_id',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '='
					}
				};
				const callback = (res) => {
					self.mainData = res.info.data[0];
					self.mainData.product[0].description = self.mainData.product[0].description.split(',');
						
					const timeList = self.$Utils.getFutureDateList(13);
					const week = self.mainData.product[0].book_week_item.split(',');
					for(var i=0; i<timeList.length; i++){
						for(var j=0; j<week.length; j++){
							if(week[j] == timeList[i].ds)
							self.timeList.push(timeList[i])
						}
					}
					self.canChooseHour = self.mainData.product[0].book_time_item.split(',');
			
					self.data.order_no = self.mainData.order_no;
					self.data.product_id = self.mainData.product_id;
					self.data.course_type = self.mainData.course_type;
					self.data.shop_no = self.mainData.shop_no;
					self.data.coach_no = self.mainData.product[0].coach_no;
					
					self.searchItem1.id = self.mainData.product_id;
					self.getProductData();
			
					uni.setStorageSync('canClick', true);
					console.log('订单详情信息',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			successSubmit(){
				const self = this;
				if(!self.choosedHour){
					console.log('请选择时间')
					self.$Utils.showToast('请选择预约时间','none')
					return;
				}
				wx.requestSubscribeMessage({
					tmplIds: [
						'BZ74KvhYwLWYKzcY-OunKaWIkbsBy_wWZ01LaZsGlKo',
						'Z5oqs6UaEnLygi7S7XSZ_dQbU71cauG0peB9qrwqrF8'
					],
					success(res) {
						console.log('res', res)
						self.submit()
					}
				});
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
				+' '+self.choosedHour;
				
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
							id:self.mainData.id
						}
					}
				];
				// if((self.mainData.orderLog.length+1)==self.mainData.standard){
				// 	postData.saveAfter[0]['data'] =  {
				// 		transport_status:2
				// 	};
				// }else{
				// 	postData.saveAfter[0]['data'] =  {
				// 		transport_status:1
				// 	};
				// }
				const callback = (res) => {
					console.log(res)
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						uni.showToast({
						    title: '预约成功',
						    duration: 2000,
						});
						setTimeout(function(){
							let pages = getCurrentPages();  //获取所有页面栈实例列表
							let nowPage = pages[ pages.length - 1];  //当前页页面实例
							let prevPage = pages[ pages.length - 2 ];  //上一页页面实例
							prevPage.$vm.page = 1;   //修改上一页data里面的searchVal参数值为1211
							uni.navigateBack({
								delta: 1
							});
						},2000)
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
<style>
page{height: 100%;}
</style>
<style scoped>
.h-120{height: 120rpx;}
.btnAuto{width: 710rpx;}

.left{width: 140rpx;text-align: center;overflow-y: auto;height: 100%;}
.left .on{background-color: #fff;color: #FF633A;}
.left .on .date{color: #FF633A;}
.right{padding: 30rpx 30rpx 80rpx;overflow-y: auto;height: 100%;}
.jl{margin-right: 68rpx;width: 140rpx;line-height: 80rpx;text-align: center;}
.jl:nth-child(3n){margin-right: 0;}

.wh26{position: absolute;top: -10rpx;right: -10rpx;}
.zzz{opacity: 0.5;background-color: #f5f5f5;}
/* .top{position: sticky;top: 0;} */
/* .bto{position: sticky;bottom: 0;} */
/* .center{padding: 160rpx 0;} */
</style>
