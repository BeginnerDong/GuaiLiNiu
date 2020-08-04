<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0">
			<view class="font-26 text-center flex1 p-2 bB-f5 time">
				<view class="wh80" v-for="(v,i) in timeList" :key="i" :class="timeCurr==i?'on':''" @click="changeTime(i)">
					<view class="pt-1">{{week[v.ds]}}</view>
					<view class="font-20 color6">{{v.m}}.{{v.d}}</view>
				</view>
			</view>
			
			<view class="font-24 line-h flex2">
				<view class="flex py-3" @click="Router.navigateTo({route:{path:'/pages/store/store'}})">
					<view>西安市</view>
					<image src="../../static/images/icon2.png" class="sj-icon"></image>
				</view>
				<view class="flex py-3" @click="Show('time')">
					<view>开课时间</view>
					<image src="../../static/images/icon3.png" class="sj-icon" v-if="time_show"></image>
					<image src="../../static/images/icon2.png" class="sj-icon" v-else></image>
				</view>
				<view class="flex py-3" @click="Show('classify')">
					<view>课程类型</view>
					<image src="../../static/images/icon3.png" class="sj-icon" v-if="class_show"></image>
					<image src="../../static/images/icon2.png" class="sj-icon" v-else></image>
				</view>
			</view>
			
			<view class="font-24 p-a bg-white px-2 w-100 tan">
				<view v-show="time_show" class="pt-4">
					<view>开课时间</view>
					<view class="d-flex a-start pt-3">
						<view class="time1 mr-5" :class="timeCurr1==0?'on':''" @click="changeTimeCurrent(0)">全天</view>
						<view class="flex1 flex-wrap flex-1 ml-4">
							<view class="time1" :class="timeCurr1==1?'on':''"
							@click="changeTimeCurrent(1)">早上 <text class="color9 font-20 pl-1">07-10点</text></view>
							<view class="time1" :class="timeCurr1==2?'on':''"
							@click="changeTimeCurrent(2)">下午 <text class="color9 font-20 pl-1">12-16点</text></view>
							<view class="time1" :class="timeCurr1==3?'on':''"
							@click="changeTimeCurrent(3)">晚上 <text class="color9 font-20 pl-1">18-21点</text></view>
						</view>
					</view>
				</view>
				<view v-show="class_show" class="pt-4">
					<view>课程类型</view>
					<view class="flex pt-3 flex-wrap classBox color6">
						<view class="time1" :class="couserType==-1?'on':''" @click="changeCourseType(-1)">全部</view>
						<block v-for="(item,index) in courseType" :key="index">
							<view class="time1" :class="couserType==index?'on':''" @click="changeCourseType(index)">{{item.title}}</view>
						</block>
						
						<!-- <view class="time1">塑型</view>
						<view class="time1">康复</view>
						<view class="time1">矫正</view>
						<view class="time1">增肌</view> -->
					</view>
				</view>
			</view>
		</view>
		
		<view class="bg-mask" style="z-index: 1;" v-show="time_show || class_show"></view>
		
		<view class="f5Bj-H20"></view>
		
		<view class="line-h px-2 py-3 flex">
			<image src="../../static/images/sijiao-icon0.png" class="dw-icon"></image>
			<view class="px-1">{{shopData.name?shopData.name:''}}</view>
			<view class="font-20 bg-f5 d-inline-block line-h-md px-1">距您：2.21KM</view>
		</view>
		<view class="shadow radius20 m-a p-r mb-3 overflow-h tkBox" 
		 @click="goToDetail(item)"
		 v-for="(item,index) in mainData" :key="index" :data-id="item.id"
		 >
			<image  :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="kcImg"></image>
			<view class="p-3">
				<view class="font-30 font-w">{{item.title?item.title:''}}</view>
				<view class="font-24 py-2">{{item.coach&&item.coach[0]?item.coach[0].name:''}} | {{item.start_time}}~{{item.end_time}}</view>
				<view class="flex">
					<view class="tag" v-for="(c_item,c_index) of item.description" :key="c_index">{{c_item}}</view>
				</view>
			</view>
			<view class="font-20 colorf kcSgin">差一个人开课</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				timeCurr:0,
				timeCurr1:0,
				couserType:-1,
				time_show:false,
				class_show:false,
				shopData:{},
				mainData:[],
				timeList:[],
				searchItem:{
					thirdapp_id:2,
					type:1,
					course_type:1
				},
				courseType:[],
				week:['周日','周一','周二','周三','周四','周五','周六'],
				chooseTimestap:0
			}
		},
		onShow(){
			const self = this;
			if(self.shopData.id!=uni.getStorageSync('shopData').id){
				self.shopData = uni.getStorageSync('shopData');
				self.getMainData(true);
			};
			console.log('show')
		},
		
		onLoad(options) {
			const self = this;
			self.timeList = self.$Utils.getFutureDateList(5);
			console.log('self.timeList',self.timeList);
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.shopData = uni.getStorageSync('shopData');
			self.chooseTimestap = self.timeList[0]['stime'];
			self.searchItem.start_time = ['between',
				[self.chooseTimestap,self.chooseTimestap+24*3600000],
			];
			
			self.$Utils.loadAll(['getMainData','getCourseTypeData'], self);
		},
		
		methods: {
			
			changeTime(i){
				const self = this;
				self.timeCurr = i;
				self.chooseTimestap = self.timeList[i]['stime'];
				self.searchItem.start_time = ['between',[self.chooseTimestap,self.chooseTimestap+86400000]];
				self.getMainData(true);
			},
			
			changeTimeCurrent(i){
				const self = this;
				self.timeCurr1 = i;
				if(i==0){
					self.searchItem.start_time = ['between',
						[self.chooseTimestap,self.chooseTimestap+24*3600000],
					];
				}else if(i==1){
					self.searchItem.start_time = ['between',
						[self.chooseTimestap+7*3600000,self.chooseTimestap+10*3600000],
					];
				}else if(i==2){
					self.searchItem.start_time = ['between',
						[self.chooseTimestap+12*3600000,self.chooseTimestap+16*3600000],
					];
				}else if(i==3){
					self.searchItem.start_time = ['between',
						[self.chooseTimestap+18*3600000,self.chooseTimestap+21*3600000],
					];
				}
				self.time_show = !self.time_show;
				self.getMainData(true);
			},
			
			changeCourseType(i){
				const self = this;
				self.couserType = i;
				if(i>=0){
					self.searchItem.category_id = uni.getStorageSync('courseType')[i].id;
				}
				self.class_show = !self.class_show;
				self.getMainData(true);
			},
			
			goToDetail(item){
				const self = this;
				uni.setStorageSync('leagueClassDetail',item);
				self.Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=0'}});
			},
			
			getShopData() {
				var self = this;
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2,
					
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.shopData = res.info.data[0];
					}
					self.$Utils.finishFunc('getShopData');
				};
				self.$apis.shopGet(postData, callback);
			},
			
			getCourseTypeData() {
				var self = this;
				if(	uni.getStorageSync('courseType')
					&&uni.getStorageSync('courseTypetTime')
					&&uni.getStorageSync('courseTypetTime')>(new Date()).getTime()
				){
					self.courseType = uni.getStorageSync('courseType');
					console.log('self.courseType',self.courseType)
					return;
				};
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2,
					parentid:6,
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.courseType = res.info.data;
						console.log('self.courseType',self.courseType)
						uni.setStorageSync('courseType', res.info.data);
						uni.setStorageSync('courseTypetTime', (new Date()).getTime()+30000);
					};
					self.$Utils.finishFunc('getCourseTypeData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				self.searchItem.shop_no = uni.getStorageSync('shopData').user_no;
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					listorder:'desc'
				};
				postData.getAfter = {
					coach:{
						tableName:'Coach',
						middleKey:'coach_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].description = self.mainData[i].description.split(',');
							self.mainData[i].start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'ymd-hms')
							self.mainData[i].end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'ymd-hms')
						}
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			
			Show(type){
				const self = this;
				if(type=='time'){
					self.time_show = !self.time_show;
					self.class_show = false;
				}else{
					self.class_show = !self.class_show;
					self.time_show = false;
				}
			}
		}
	}
</script>

<style scoped>
.time .on{background-color: #FF633A;border-radius: 50%; color: #fff!important;}
.time .on view{color: #fff;}
</style>
