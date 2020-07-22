<template>
	<view>
		
		<view class="txt text-center">
			<view class="colorf pb-2">如需重新修改，请联系管理员删除</view>
			<view class="colorM" @click="Router.navigateTo({route:{path:'/pages/faceApply/faceApply'}})">申请删除</view>
		</view>
		
		<view class="photo p-r" @click="isShow">
			<image src="../../static/images/face recogontion-icon.png"></image>
			<view class="wh300 radius-5 Mgb p-aXY">
				<image src="../../static/images/face recogontion-icon1.png" class="img"></image>
			</view>
		</view>
		
		<view class="bg-mask" v-show="is_show">
			<view class="bg-white p-aX bottom-0 text-center">
				<view class="py-3">拍照</view>
				<view class="py-3 bT-e1">从相册中选择</view>
				<view class="f5Bj-H20"></view>
				<view class="py-3 bT-e1" @click="isShow">取消</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show:false
			}
		},
		methods: {
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			}
		}
	}
</script>

<style>
page{background-color: #000000;}
</style>
<style scoped>
.txt{margin-top: 110rpx;}
.photo{width: 440rpx;height: 440rpx;margin: 130rpx auto 0;}
.img{width: 109rpx;height: 92rpx;position: absolute;margin: auto;left: 0;top: 0;right: 0;bottom: 0;}
.wh300{margin: 70rpx;}
</style>