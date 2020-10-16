<template>
	<view>
		
		<view class="px-2 flex1 flex-wrap pb-5">
			
			<view class="p-r overflow-h line-h radius10 shadowM mt-3 sj" v-for="(item,index) of mainData"
			:key="item.id"
			:data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/sijiao-detail/sijiao-detail?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="sjImg1"></image>
				<view class="px-2">
					<view class="font-w py-2 txt">{{item.name}}</view>
					<view class="flex">
						<image src="../../static/images/sijiao-icon.png" class="bq-icon"></image>
						<view class="font-22 color6 pl-1">{{item.expertise}}</view>
					</view>
					<view class="flex1 pb-2 pt-3">
						<view class="font-26 colorR"><text class="price">{{item.class&&item.class[0]?item.class[0].price:'0'}}</text>/节</view>
						<view class="font-20 color9">累计 {{item.course?item.course.num:0}}节</view>
					</view>
				</view>
				<view class="sjSgin">{{item.certificate.length}}项专业证书</view>
			</view>
			
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				searchItem:{
					thirdapp_id:2
				},
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
			// self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.shop_no = options.shop_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
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
				postData.getAfter = {
					class:{
						tableName:'Product',
						middleKey:'user_no',
						key:'coach_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					course:{
						tableName:'Course',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1,
							is_book:1
						},
						condition:'=',
						compute:{
							num:[
								'count',
								'count',
								{status:1,is_book:1}
							]
						}
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.coachGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.sj{width: 340rpx;}
.sjImg1{width: 340rpx;height: 200rpx;}
.sjSgin{font-size: 22rpx;color: #fff;line-height: 36rpx;padding: 0 8rpx;background-color: rgba(0,0,0,0.3);display: inline-block;position: absolute;right: 10rpx;top: 154rpx;border-radius: 5rpx;}

.txt{min-height: 68rpx;}

.bq-icon{width: 25rpx;height: 27rpx;}
</style>
