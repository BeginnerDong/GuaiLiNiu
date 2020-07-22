<template>
	<view>
		
		<view class="Mgb px-2 pt-3 colorf text-center line-h">
			<image src="../../static/images/to join in-img.png" class="logo"></image>
			<view class="font-60 pt-5">健身就选怪力牛运动</view>
			<view class="font-36 pt-3 pb-5">提供最好的服务、最好就业条件</view>
			<view class="flex0">
				<view class="text" v-for="v in text">{{v}}</view>
			</view>
			<image src="../../static/images/to join in-img1.png" class="img1"></image>
		</view>
		
		<view class="ru px-2 pb-5">
			<view class="font-36 line-h pt-5 pb-2 text-center p-r mb-4 tit">申请入口</view>
			<input type="text" value="" placeholder="请输入您的姓名" />
			<input type="text" value="" placeholder="请输入您的手机号" />
			<input type="text" value="" placeholder="请输入您选择的区域" />
			<view class="btnAuto">确定</view>
		</view>
		
		<view class="px-2">
			<view class="font-36 line-h pt-5 pb-2 text-center p-r mb-4 tit">加盟条件</view>
			
			<view class="font-26 mb-3 d-flex a-start">
				<view class="p-r flex">
					<image src="../../static/images/to join in-icon.png" class="joinIcon"></image>
					<view class="xh">1</view>
				</view>
				<view class="pl-2 flex-1">
					<view class="colorM">强大的品牌</view>
					<view>爱迪生肯定会将凯德菲萨克的撒萨利收快递费聚少离多看的阿了山东矿机啥抵扣卷啥抵扣卷阿啥抵扣卷阿SDK滤镜大富科技打开吗靠父母了快递费吗了山东矿机手机壳拉斯了四大皆空拉看电视剧可视对讲</view>
				</view>
			</view>
			
			<view class="pb-5 overflow-h">
				<image src="../../static/images/to join in-img2.png" class="img2"></image>
				<image src="../../static/images/to join in-img2.png" class="img2"></image>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				text:['专','业','一','对','一','服','务']
			}
		},
		methods: {
			
		}
	}
</script>

<style scoped>
.logo{width: 196rpx;height: 52rpx;}
.text{width: 50rpx;line-height: 50rpx;border-radius: 50%;color: #222;font-size: 30rpx;text-align: center;background-color: #FFCDC0;margin: 0 15rpx;}
.img1{width: 413rpx;height: 463rpx;margin: auto;}
.ru{background-color: #FFF3EB;}
.tit::before{content: '';background-color: #FF633A;width: 60rpx;height: 4rpx;position: absolute;bottom: 0;left: 50%;margin-left: -25rpx;}
input{width: 710rpx;height: 80rpx;font-size: 26rpx;border: 1px solid #e1e1e1;background-color: #fff;border-radius: 10rpx;margin-bottom: 30rpx;text-indent: 20rpx;}

.joinIcon{width: 38rpx;height: 34rpx;}

.xh{color: #fff;position: absolute;top: 0;left: 0;line-height: 34rpx;width: 38rpx;text-align: center;}
.img2{width: 500rpx;height: 300rpx;}
.img2:nth-child(2){margin-top: -80rpx;float: right;}
</style>
