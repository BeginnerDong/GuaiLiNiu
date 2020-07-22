<template>
	<view>
		
		<image src="../../static/images/sijiao-img1.png" class="sjImg"></image>
		<view class="py-3 flex1 px-2">
			<view class="line-h flex">
				<image src="../../static/images/sijiao-icon0.png" class="dw-icon"></image>
				<view class="px-1 font-w">西安外事学院</view>
				<view class="font-20 bg-f5 d-inline-block line-h-md px-1">距您：2.21KM</view>
			</view>
			<view class="flex" @click="Router.navigateTo({route:{path:'/pages/sijiao-store/sijiao-store'}})">
				<view class="color6">选择门店</view>
				<image src="../../static/images/the order-icon3.png" class="R-icon ml-2"></image>
			</view>
		</view>
		
		<view class="flex shadowM mx-2 radius10 py-3 tan">
			<image src="../../static/images/sijiao-icon2.png" class="wh40 mx-2"></image>
			<view class="color6">优选课程</view>
			<view class="flex flex-1 pl-5">
				<view class="time1">减脂</view>
				<view class="time1">增肌</view>
			</view>
			<view class="time1 on" @click="Router.navigateTo({route:{path:'/pages/sijiao-course/sijiao-course'}})">全部</view>
		</view>
		
		<view class="px-2 flex1 flex-wrap pb-5">
			
			<view class="p-r overflow-h line-h radius10 shadowM mt-3 sj" @click="Router.navigateTo({route:{path:'/pages/sijiao-detail/sijiao-detail'}})">
				<image src="../../static/images/sijiao-img.png" class="sjImg1"></image>
				<view class="px-2">
					<view class="font-w py-2">张瑞生</view>
					<view class="flex">
						<image src="../../static/images/sijiao-icon.png" class="bq-icon"></image>
						<view class="font-22 color6 pl-1">极速瘦身、减脂塑形</view>
					</view>
					<view class="flex1 py-2">
						<view class="font-26"><text class="price">220</text>/节</view>
						<view class="font-20 color9">累计 105节</view>
					</view>
				</view>
				<view class="sjSgin">1项专业证书</view>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
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
