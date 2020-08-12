<template>
	<view>
		<block v-for="(item,index) in mainData" :key="index">
			<view class="shadow radius20 m-a p-r mb-3 tkBox" @click="goToDetail(item)">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="kcImg"></image>
				<view class="px-2 py-3 font-30 line-h">
					<view class="font-w pb-3">{{item.title}}</view>
					<view><text class="price font-w">{{item.price}}</text>/{{item.score}}课时</view>
				</view>
			</view>
		</block>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				searchItem:{
					coach_no:''
				}
				
			}
		},
		onLoad(options) {
			const self = this;
			self.searchItem.coach_no = options.coach_no;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			goToDetail(item){
				const self = this;
				uni.setStorageSync('sijiaoCourseDetail',item);
				self.Router.navigateTo({route:{path:'/pages/sijiao-classDetail/sijiao-classDetail'}})
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
						condition:'=',
						info:['name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	}
</script>

<style>

</style>
