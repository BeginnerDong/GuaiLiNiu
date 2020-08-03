<template>
	<view>
		
		<!-- banner -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff" class="banner">
			<swiper-item>
				<image src="../../static/images/pay for courses-img.png" ></image>
			</swiper-item>
		</swiper>
		
		<view class="content bg-white radius30-T px-2 p-r content">
			<view class="pb-4 bB-f5">
				<view class="flex pt-5 font-40 font-w">时空穿梭</view>
				<view class="shareSgin" @click="Router.navigateTo({route:{path:'/pages/limitedTime/limitedTime'}})">
					<image src="../../static/images/course-icon2.png" class="fx-icon"></image>
					<view>分享</view>
				</view>
				<view class="flex pt-3">
					<view class="bR-e1 pr-2 flex">
						<image src="../../static/images/pay for courses-icon.png" class="wh30 mr-1"></image>
						团操课
					</view>
					<view class="flex pl-2">
						<view class="tag tagY">矫正</view>
						<view class="tag tagB">提升柔软度</view>
						<view class="tag tagG">改善身体线条</view>
					</view>
				</view>
				
				<!-- 付费团课展示 -->
				<view class="font-20 pt-3"><text class="font-36 font-w price">220</text>/9课时</view>
			</view>
			
			<view class="font-26 pt-4 ">
				<view class="d-flex a-start pb-4">
					<image src="../../static/images/pay for courses-icon1.png" class="rq-icon mt"></image>
					<view class="flex-1 pl-2">适合人群：广大瘦身小白</view>
				</view>
				<view class="d-flex a-start pb-4">
					<image src="../../static/images/members-icon6.png" class="fw-icon mt"></image>
					<view class="flex-1 pl-2">服务教练：张思雨</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="p-3 radius10 shadowM p-r">
				<view class="font-w t-indent20 font-30 line-h tit">课程内容</view>
				<view class="font-26 color9 pt-3">
					你自己东方红快递费课程VB好歹加了挡风叫代驾看信访局看信访局看打扫房间墨刀上繁花似锦看能吃就树倒猢狲散可视电话咋SD卡和v时代剧华可视电话不打算看三等奖发发沙迪克v和阿啥抵扣卷
				</view>
				<view class="p-a flex1 left-0 right-0 mt-2 px-2">
					<image src="../../static/images/course-icon.png" class="courIcon"></image>
					<image src="../../static/images/course-icon1.png" class="courIcon"></image>
				</view>
			</view>
			<view class="p-3 radius10 shadowM p-r mt-2">
				<view class="font-w t-indent20 font-30 line-h tit">课程规则</view>
				<view class="font-26 color9 pt-3">
					你自己东方红快递费课程VB好歹加了挡风叫代驾看信访局看信访局看打扫房间墨刀上繁花似锦看能吃就树倒猢狲散可视电话咋SD卡和v时代剧华可视电话不打算看三等奖发发沙迪克v和阿啥抵扣卷
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="font-w t-indent20 font-30 line-h tit">约课流程</view>
			<view class="font-24 flex1 pt-4">
				<view class="flex1 flex-1 pr-5">
					<view class="LC flex4">
						<image src="../../static/images/pay for courses-icon5.png"></image>
						<view>1.购买</view>
					</view>
					<view class="LC flex4">
						<image src="../../static/images/pay for courses-icon6.png"></image>
						<view>2.约课</view>
					</view>
					<view class="LC flex4">
						<image src="../../static/images/pay for courses-icon7.png"></image>
						<view>3.签到</view>
					</view>
					<view class="LC flex4">
						<image src="../../static/images/pay for courses-icon8.png"></image>
						<view>4.评价</view>
					</view>
				</view>
				<view class="flex4 colorM pl-5 p-r GZ">
					<image src="../../static/images/pay for courses-icon9.png" class="lcIcon5"></image>
					<view>规则详情</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="font-w t-indent20 font-30 line-h tit">用户评价</view>
			<view class="bB-f5 py-3">
				<view class="font-24 flex1">
					<image src="../../static/images/pay for courses-img3.png" class="wh70"></image>
					<view class="color6 flex-1 px-2">用户名</view>
					<view class="color9">2020.02.23</view>
				</view>
				<view class="font-26 pt-3">
					很愉快的一次购物，非常满意，很愉快的一次购物，非常满意，很愉快的一次购物，非常满意，
				</view>
				<view class="flex flex-wrap">
					<image src="../../static/images/pay for courses-img4.png" class="wh180 mt-2 mr-2"></image>
					<image src="../../static/images/pay for courses-img4.png" class="wh180 mt-2 mr-2"></image>
				</view>
			</view>
		</view>
		
		
		<view style="height: 100rpx;"></view>
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 p-2 bT-e1">
			<view class="font-26">已预约0/5人，还差<text class="colorR">3</text>人开课</view>
			<view class="criBtn" @click="Router.navigateTo({route:{path:'/pages/sijiao-order/sijiao-order'}})">立即购买</view>
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
.banner{width: 100%;height: 580rpx;}
.content{margin-top: -50rpx;}
.rq-icon{width: 32rpx;height: 28rpx;}
.fw-icon{width: 36rpx;height: 29rpx;}
.jlImg{width: 180rpx;height: 220rpx;border-radius: 10rpx;}
.tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #FF633A;position: absolute;left: 20rpx;}
.LC image{width: 52rpx;height: 48rpx;margin-bottom: 30rpx;}
.lcIcon5{width: 40rpx;height: 44rpx;margin-bottom: 30rpx;}
.GZ::before{content: "";border-left: 1px solid #e1e1e1;position: absolute;left: 0;height: 60rpx;}
.courIcon{width: 26rpx;height: 43rpx;}
.criBtn{line-height: 80rpx;border-radius: 40rpx;margin: 0;width: 260rpx;}

.noVip{width: 620rpx;margin-top: 65%;}
</style>
