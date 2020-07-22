<template>
	<view>
		
		<view class="px-2 flex1 flex-wrap pb-5">
			
			<view class="p-r overflow-h line-h radius10 shadowM mt-3 sj" @click="Router.navigateTo({route:{path:'/pages/sijiao-detail/sijiao-detail'}})">
				<image src="../../static/images/sijiao-img.png" class="sjImg1"></image>
				<view class="px-2">
					<view class="font-w py-2">张瑞生</view>
					<view class="flex">
						<image src="../../static/images/sijiao-icon.png" class="bq-icon"></image>
						<view class="font-22 color6 pl-1">极速瘦身、减脂塑形</view>
					</view>
					<view class="flex1 pb-2 pt-3">
						<view class="font-26 colorR"><text class="price">220</text>/节</view>
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
.sj{width: 340rpx;}
.sjImg1{width: 340rpx;height: 200rpx;}
.sjSgin{font-size: 22rpx;color: #fff;line-height: 36rpx;padding: 0 8rpx;background-color: rgba(0,0,0,0.3);display: inline-block;position: absolute;right: 10rpx;top: 154rpx;border-radius: 5rpx;}

.bq-icon{width: 25rpx;height: 27rpx;}
</style>
