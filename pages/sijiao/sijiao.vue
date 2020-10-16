<template>
	<view>
		
		<image src="../../static/images/sijiao-img1.png" class="sjImg"></image>
		<view class="py-3 flex1 px-2">
			<view class="line-h flex">
				<image src="../../static/images/sijiao-icon0.png" class="dw-icon"></image>
				<view class="px-1 font-w">{{shopData.name}}</view>
				<view class="font-20 bg-f5 d-inline-block line-h-md px-1">距您：{{shopData.distance}}KM</view>
			</view>
			<view class="flex" @click="Router.navigateTo({route:{path:'/pages/sijiao-store/sijiao-store'}})">
				<view class="color6">选择门店</view>
				<image src="../../static/images/the-order-icon3.png" class="R-icon ml-2"></image>
			</view>
		</view>
		
		<view class="flex shadowM mx-2 radius10 py-3 tan" @click="Router.navigateTo({route:{path:'/pages/sijiao-course/sijiao-course?shop_no='+shopData.user_no}})">
			<image src="../../static/images/sijiao-icon2.png" class="wh40 mx-2"></image>
			<view class="color6">优选课程</view>
			<view class="flex flex-1 pl-5">
				<view class="time1">减脂</view>
				<view class="time1">增肌</view>
			</view>
			<view class="time1 on">全部</view>
		</view>
		
		<view class="px-2 flex1 flex-wrap pb-5">
			<block v-for="(item,index) in mainData">
				<view class="p-r overflow-h line-h radius10 shadowM mt-3 sj"
				@click="goDetail(item)">
					<image :src="item.mainImg[0].url" class="sjImg1"></image>
					<view class="px-2">
						<view class="font-w py-2">{{item.name}}</view>
						<view class="flex">
							<image src="../../static/images/sijiao-icon.png" class="bq-icon"></image>
							<view class="font-22 color6 pl-1">{{item.expertise}}</view>
						</view>
						<view class="flex1 py-2">
							<view class="font-26"><text class="price">{{item.products.price}}</text>/节</view>
							<view class="font-20 color9">累计 {{item.class}}节</view>
						</view>
					</view>
					<view class="sjSgin">{{item.certificate.length}}项专业证书</view>
				</view>
			</block>
			
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				shopData:{},
				mainData:[],
				searchItem:{
					
				},
				isLoadAll:false,
				paginate: {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				}
			}
		},
		onLoad(options) {
			const self = this;
			self.shopData = uni.getStorageSync('shopData');
			// self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.shop_no = self.shopData.user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		onShow() {
			const self = this;
			if(self.shopData.id != uni.getStorageSync('shopData').id){
				self.shopData = uni.getStorageSync('shopData');
				self.searchItem.shop_no = self.shopData.user_no;
				self.getMainData(true)
			}
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			goDetail(item){
				const self = this;
				self.Router.navigateTo({route:{path:'/pages/sijiao-detail/sijiao-detail?coach_no='+item.user_no}})
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					products:{
						tableName:'Product',
						middleKey:'user_no',
						key:'coach_no',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['price']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							// self.mainData[i].expertise = self.mainData[i].expertise.split(',')
						}
					}else{
						self.isLoadAll = true;
					};
					uni.setStorageSync('canClick', true);
					console.log('mainData',self.mainData)
					// console.log('products',products)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.coachGet(postData, callback);
			}
			
		}
	}
</script>

<style scoped>
.sjImg{height: 180rpx;}
.time1{border: 1px solid #FF633A;color: #FF633A;margin: 0 20rpx 0 0;font-size: 22rpx;line-height: 36rpx;padding: 0 30rpx;}
.tan .on{color: #fff;}

.sj{width: 340rpx;}
.sjImg1{width: 340rpx;height: 200rpx;}
.sjSgin{font-size: 22rpx;color: #fff;line-height: 36rpx;padding: 0 8rpx;background-color: rgba(0,0,0,0.3);display: inline-block;position: absolute;right: 10rpx;top: 154rpx;border-radius: 5rpx;}

.bq-icon{width: 25rpx;height: 27rpx;}
</style>
