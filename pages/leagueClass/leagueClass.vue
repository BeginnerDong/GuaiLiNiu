<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0">
			<view class="font-26 text-center flex1 p-2 bB-f5 time">
				<view class="wh80" v-for="(v,i) in 6" :class="timeCurr==i?'on':''" @click="changeTime(i)">
					<view class="pt-1">今天</view>
					<view class="font-20 color6">6.24</view>
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
						<view class="time1 mr-5 on">全天</view>
						<view class="flex1 flex-wrap flex-1 ml-4">
							<view class="time1">早上 <text class="color9 font-20 pl-1">07-10点</text></view>
							<view class="time1">下午 <text class="color9 font-20 pl-1">07-10点</text></view>
							<view class="time1">晚上 <text class="color9 font-20 pl-1">07-10点</text></view>
						</view>
					</view>
				</view>
				<view v-show="class_show" class="pt-4">
					<view>课程类型</view>
					<view class="flex pt-3 flex-wrap classBox color6">
						<view class="time1 on">全部</view>
						<view class="time1">减脂</view>
						<view class="time1">塑型</view>
						<view class="time1">康复</view>
						<view class="time1">矫正</view>
						<view class="time1">增肌</view>
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
				time_show:false,
				class_show:false,
				shopData:{},
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					type:1,
					course_type:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.shopData = uni.getStorageSync('shopData');
			self.searchItem.shop_no = uni.getStorageSync('shopData').user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
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
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
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
							self.mainData[i].start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'hm')
							self.mainData[i].end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'hm')
						}
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			changeTime(i){
				const self = this;
				self.timeCurr = i
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
