<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0">
			<view class="font-24 line-h flex2 bB-f5">
				<view class="flex py-3" @click="Router.navigateTo({route:{path:'/pages/store/store'}})">
					<view>西安市</view>
					<image src="../../static/images/icon2.png" class="sj-icon"></image>
				</view>
				<view class="flex py-3" @click="Show()">
					<view>课程类型</view>
					<image src="../../static/images/icon3.png" class="sj-icon" v-if="class_show"></image>
					<image src="../../static/images/icon2.png" class="sj-icon" v-else></image>
				</view>
			</view>
			
			<view class="font-24 p-a bg-white px-2 w-100 tan">
				<view v-show="class_show" class="pt-4">
					<view>课程类型</view>
					<view class="flex pt-3 flex-wrap classBox color6">
						<view class="time1" :class="courseCurr==-1?'on':''" @click="changeCourseType(-1)">全部</view>
						<block v-for="(item,index) in courseType" :key="index">
							<view class="time1" :class="courseCurr==index?'on':''" @click="changeCourseType(index)">{{item.title}}</view>
						</block>
					</view>
				</view>
			</view>
		</view>
		<view class="bg-mask" style="z-index: 1;" v-show="time_show || class_show"></view>
		
		
		
		<view class="line-h px-2 py-3 flex">
			<image src="../../static/images/sijiao-icon0.png" class="dw-icon"></image>
			<view class="px-1">{{shopData.name}}</view>
			<view class="font-20 bg-f5 d-inline-block line-h-md px-1">距您：2.21KM</view>
		</view>
		<view class="shadow radius20 m-a p-r mb-3 tkBox" v-for="(item,index) in mainData" :key="index" @click="goToDetail(item)">
			<image src="../../static/images/sijiao-img.png" class="kcImg"></image>
			<view class="px-2 py-3">
				<view class="font-30 flex1">
					<view class="font-w">{{item.title}}</view>
					<view><text class="price font-w">{{item.price}}</text>/{{item.score}}课时</view>
				</view>
				<view class="font-24 py-2">{{item.coach[0].name}} | {{item.start_time}}~{{item.end_time}}</view>
				<view class="flex">
					<view class="tag tagY" v-for="(v,i) in item.description" :key="i">{{v}}</view>
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
				class_show:false,
				mainData:[],
				shopData:{},
				searchItem:{
					thirdapp_id:2,
					type:1,
					course_type:2
				},
				courseCurr:-1,
				courseType:[]
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.shop_no = uni.getStorageSync('shopData').user_no;
			self.shopData = uni.getStorageSync('shopData');
			self.$Utils.loadAll(['getMainData','getCourseTypeData'], self);
		},
		methods: {
			
			goToDetail(item){
				const self = this;
				uni.setStorageSync('leagueClassDetail',item);
				self.Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=1'}});
			},
			
			changeCourseType(i){
				const self = this;
				self.courseCurr = i;
				if(i>=0){
					self.searchItem.category_id = uni.getStorageSync('courseType')[i].id;
				}else{
					delete self.searchItem.category_id;
				};
				self.class_show = !self.class_show;
				self.getMainData(true);
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
					
					console.log('self.courseType',self.courseType)
					self.$Utils.finishFunc('getCourseTypeData');
				};
				self.$apis.labelGet(postData, callback);
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
					console.log('mainData',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			Show(){
				const self = this;
				self.class_show = !self.class_show;
			}
		}
	}
</script>

<style>

</style>
