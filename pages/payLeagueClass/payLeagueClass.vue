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
		
		
		
		<view class="line-h px-2 py-3 flex">
			<image src="../../static/images/sijiao-icon0.png" class="dw-icon"></image>
			<view class="px-1">西安外事学院</view>
			<view class="font-20 bg-f5 d-inline-block line-h-md px-1">距您：2.21KM</view>
		</view>
		<view class="shadow radius20 m-a p-r mb-3 tkBox" @click="Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=1'}})">
			<image src="../../static/images/sijiao-img.png" class="kcImg"></image>
			<view class="px-2 py-3">
				<view class="font-30 flex1">
					<view class="font-w">减脂训练营·中上</view>
					<view><text class="price font-w">220</text>/9课时</view>
				</view>
				<view class="font-24 py-2">Auger | 20:00~20:45</view>
				<view class="flex">
					<view class="tag tagY">矫正</view>
					<view class="tag tagB">提升柔软度</view>
					<view class="tag tagG">改善身体线条</view>
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
				searchItem:{
					thirdapp_id:2,
					type:1,
					course_type:2
				}
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.shop_no = uni.getStorageSync('shopData').user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
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
							self.mainData[i].start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'yy-hh-mm')
							self.mainData[i].end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'hm')
						}
					};
					uni.setStorageSync('canClick', true);
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
