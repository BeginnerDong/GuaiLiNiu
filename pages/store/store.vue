<template>
	<view class="h-100">
		
		<view class="font-24 flex1 px-2 bg-white store">
			
			<view class="flex-1 flex1 my-2">
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
<style>
page{height: 100%;}
</style>
<style scoped>
.storeImg{width: 230rpx;height: 160rpx;}
.store .dz{width: 400rpx;padding: 20rpx 0;}
.sq{top: -10rpx;right: -10rpx;}
.zz{margin-top: 100rpx;}
</style>
