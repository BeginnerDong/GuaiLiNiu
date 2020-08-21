<template>
	<view>
		
		<view class="flexX colorf font-24 mx-2 py-3 kcBox">
			<view class="kc  flex-shrink flex4 shadow" :class="current==-1?'on':''" @click="change(-1)">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-1.png" class="sjkIcon1"></image>
				</view>
				<view>全部</view>
			</view>
			<view class="kc flex-shrink flex4 shadow" v-for="(item,index) in menuData"  
			:key="item.id"  :class="current==index?'on':''"  @click="change(index)">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image :src="item.mainImg[0].url" class="sjkIcon1"></image>
				</view>
				<view>{{item.title}}</view>
			</view>
		</view>
		
		
		<view class="mx-2 mb-2 p-r line-h radius10 overflow-h" v-for="(item,index) of mainData" :key="item.id" @click="goToDetail(item)">
			<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="sjkImg"></image>
			<view class="colorf p-aXY pt-5 px-4 sjkBg">
				<view class="font-44 font-w pt-5 py-4">{{item.title}}</view>
				<view><text class="price">{{item.price}}</text>/课时起</view>
			</view>
		</view>

		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				menuData:[],
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					type:1
				},
				current:-1
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			// self.searchItem.user_no = options.shop_no;
			// console.log('shop_no',self.searchItem.shop_no)
			self.$Utils.loadAll(['getMenuData','getMainData'], self);
		},
		methods: {
			
			goToDetail(item){
				const self = this;
				if(item.course_type==3){
					uni.setStorageSync('sijiaoCourseDetail',item);
					self.$Router.navigateTo({route:{path:'/pages/sijiao-classDetail/sijiao-classDetail'}});
				}else{
					uni.setStorageSync('leagueClassDetail',item);
					self.$Router.navigateTo({route:{path:'/pages/leagueClass-detail/leagueClass-detail?type=1'}});
				};
			},
			
			change(index){
				const self = this;
				if(self.current != index){
					self.current = index;
					if(self.current==-1){
						delete self.searchItem.category_id
					}else{
						self.searchItem.category_id = self.menuData[index].id
					};
					self.getMainData(true)
				}
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				self.searchItem.user_no = uni.getStorageSync('shopData').user_no;
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
							self.mainData[i].start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'ymd-hm')
							self.mainData[i].end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'ymd-hm')
						}
					}else{
						self.isLoadAll = true;
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},

			getMenuData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					article: {
						tableName: 'Label',
						middleKey: 'parentid',
						key: 'id',
						searchItem: {
							title: ['in', ['团课']],
						},
						condition: 'in'
					}
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.menuData = res.info.data;
					}
					console.log('sijiao',self.menuData)
					self.$Utils.finishFunc('getMenuData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.kc{width: 100rpx;height: 140rpx;border-radius: 50rpx;background-color: #ddd;margin-right: 40rpx;}
.kcBox .on{background-color: #FF633A;}
.sjkIcon1{width: 30rpx;height: 38rpx;-webkit-filter: grayscale(100%); -moz-filter: grayscale(100%);-ms-filter: grayscale(100%);-o-filter: grayscale(100%); filter: grayscale(100%); filter: gray(100%);}
.sjkImg{width: 710rpx;height: 320rpx}
.kcBox .on .sjkIcon1{;-webkit-filter: grayscale(0); -moz-filter: grayscale(0);-ms-filter: grayscale(0);-o-filter: grayscale(0); filter: grayscale(0); filter: gray(0)}
.price{color: #fff;}
.sjkBg{background-color: rgba(0,0,0,0.2);}
</style>
