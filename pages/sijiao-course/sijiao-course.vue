<template>
	<view>
		
		<view class="flexX colorf font-24 mx-2 py-3 kcBox">
			<view class="kc on flex-shrink flex4 shadow">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-1.png" class="sjkIcon1"></image>
				</view>
				<view>全部</view>
			</view>
			<view class="kc flex-shrink flex4 shadow">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-2.png" class="sjkIcon1"></image>
				</view>
				<view>减脂</view>
			</view>
			<view class="kc flex-shrink flex4 shadow">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-3.png" class="sjkIcon1"></image>
				</view>
				<view>增肌</view>
			</view>
			<view class="kc flex-shrink flex4 shadow">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-4.png" class="sjkIcon1"></image>
				</view>
				<view>塑型</view>
			</view>
			<view class="kc flex-shrink flex4 shadow">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-5.png" class="sjkIcon1"></image>
				</view>
				<view>康复</view>
			</view>
			<view class="kc flex-shrink flex4 shadow">
				<view class="bg-white radius-5 wh80 flex0 mb-1">
					<image src="../../static/images/course-icon-5.png" class="sjkIcon1"></image>
				</view>
				<view>康复</view>
			</view>
		</view>
		
		
		<view class="mx-2 mb-2 p-r line-h radius10 overflow-h">
			<image src="../../static/images/course-img-1.png" class="sjkImg"></image>
			<view class="colorf p-aXY pt-5 px-4 sjkBg">
				<view class="font-44 font-w pt-5 py-4">凸出腹部马甲线</view>
				<view><text class="price">220</text>/课时起</view>
			</view>
		</view>
		<view class="mx-2 mb-2 p-r line-h radius10 overflow-h">
			<image src="../../static/images/course-img-1.png" class="sjkImg"></image>
			<view class="colorf p-aXY pt-5 px-4 sjkBg">
				<view class="font-44 font-w pt-5 py-4">凸出腹部马甲线</view>
				<view><text class="price">220</text>/课时起</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.kc{width: 100rpx;height: 140rpx;border-radius: 50rpx;background-color: #ddd;margin-right: 40rpx;}
.kcBox .on{background-color: #FF633A;}
.sjkIcon1{width: 30rpx;height: 38rpx;}
.sjkImg{width: 710rpx;height: 320rpx;}
.price{color: #fff;}
.sjkBg{background-color: rgba(0,0,0,0.2);}
</style>
