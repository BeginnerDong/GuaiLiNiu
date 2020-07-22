<template>
	<view>
		
		<view class="p-r">
			<image src="../../static/images/img2.png" class="img"></image>
			
			<view class="con colorf p-aXY">
				<view>
					1、激活成功可享受7天的会员权益
				</view>
				
				<view class="btnTxt p-aX" @click="Router.navigateTo({route:{path:'/pages/experienceInfor/experienceInfor'}})">立即激活</view>
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

<style>
page{background-color: #63D1F8;}
</style>
<style scoped>
.img{width: 630rpx;height: 956rpx;margin: 15% auto;}
.con{margin: 60% 112rpx 0;}
.btnTxt{text-align: center;bottom: 30%;}
</style>
