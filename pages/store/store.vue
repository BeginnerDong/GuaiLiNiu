<template>
	<view class="h-100">
		
		<view class="font-24 px-2 bg-white storeBox" >
			<block v-for="(item,index) in mainData" :key="index">
				<view class="store" @click="goBack(item)">
					<view class="my-2 flex1">
						<image src="../../static/images/stores-img.png" class="storeImg"></image>
						<view class="flex3 pl-2 flex-1">
							<view class="flex">
								<view class="font-34">西安外事学院</view>
								<view class="font-20 colorf p-r sq">
									<image src="../../static/images/home-icon5.png" class="sq-icon"></image>
									<view class="top-0 p-a t-indent10 line-h-md">社区店</view>
								</view>
							</view>
							<view class="color6 avoidOverflow dz">西安市雁塔区鱼斗路61号左岸春天A座101002室2楼</view>
							<view>距离：2.23KM</view>
						</view>
						<image src="../../static/images/stores-icon1.png" class="wh33 zz"></image>
					</view>
				</view>
			</block>
		</view>
	
		
		
		<view class="flex0 mt-5 py-4 color9">
			<view class="line"></view>
			<view class="px-4">我是有底线的</view>
			<view class="line"></view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					user_type:1
				}
			}
		},
		onLoad() {
			const self = this;
			var shopTime = uni.getStorageSync('shopListTime');
			if(shopTime&&uni.getStorageSync('shopList')&&shopTime>(new Date()).getTime()){
				self.mainData = uni.getStorageSync('shopList');
				console.log('self.mainData',self.mainData)
			}else{
				self.$Utils.loadAll(['getMainData'], self);
			};
			
			
			
		},
		methods: {
			
			goBack(item){
				const self = this;
				uni.setStorageSync('shopData',item);
				uni.navigateBack({
				    delta: 1
				});
			},
			
			getMainData(){
				const self = this;
				uni.getLocation({
					type:'wgs84',
					success(res) {
						self.getShopData(res.longitude,res.latitude)
					}
				})
			},
			
			getShopData(longitude,latitude) {
				var self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2,
					user_type:1
				};
				postData.order = {
					distance:'asc',
					longitude:longitude,
					latitude:latitude
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.mainData.push.apply(self.mainData,res.info.data);	
						console.log('self.mainData',self.mainData)
						uni.setStorageSync('shopList', res.info.data);
						uni.setStorageSync('shopListTime', (new Date()).getTime()+30000);
					}else{
						uni.showModal({
							title:'提示',
							content:'更多门店筹备中...',
							showCancel:false
						})
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.shopGet(postData, callback);
			},
		}
	}
</script>
<style>
page{height: 100%;}
</style>
<style scoped>
.storeBox{min-height: 80%;}
.storeImg{width: 230rpx;height: 160rpx;}
.store .dz{width: 400rpx;padding: 20rpx 0;}
.sq{top: -10rpx;right: -10rpx;}
.zz{margin-top: 100rpx;}
.line{height: 2rpx;background-color: #e1e1e1;width: 100rpx;}
</style>
