<template>
	<view class="h-100" @click="fun">
		
		<image src="../../static/images/icon.png" class="startTxt animated zoomIn"></image>
		<image src="../../static/images/icon1.png" class="startLogo animated zoomIn"></image>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				time:null
			}
		},
		onLoad(){
			const self = this;
			self.timer();
		},
		methods: {
			timer(){
				const self = this;
				self.time = setTimeout(function(){
					uni.redirectTo({
						url:'/pages/index/index'
					})
				},2000)
			},
			fun(){
				const self = this;
				clearTimeout();
				self.timer();
				self.time = null;
			}
		},
		destroyed() {
			const self = this;
			clearTimeout();
			self.time = null;
		}
	}
</script>

<style>
page{height: 100%;}
.startTxt{width: 65rpx; height: 610rpx;margin: 35% auto 0;}
.startLogo{width: 358rpx;height: 96rpx;margin: 0 auto 20%;position: fixed;bottom: 0;left: 0;right: 0;}
</style>
