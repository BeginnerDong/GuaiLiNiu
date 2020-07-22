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
					<view class="flex-1 pl-2">适合人群：矫正、想要提升柔软度、改善身体线条、减肥、长期久坐、不爱运动的人群</view>
				</view>
				<view class="d-flex a-start pb-4">
					<image src="../../static/images/pay for courses-icon2.png" class="wh30 mt"></image>
					<view class="flex-1 pl-2">预计<text class="colorR">6月24日</text>开课<br />每周一/三/五 19:00~20:00</view>
				</view>
				<view class="d-flex a-start pb-4">
					<image src="../../static/images/pay for courses-icon3.png" class="dw-icon mt"></image>
					<view class="flex-1 pl-2">适合人群：矫正、想要提升柔软度、改善身体线条、减肥、长期久坐、不爱运动的人群</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-4">
			<view class="font-30 font-w">服务教练</view>
			<view class="flex1 pt-3">
				<image src="../../static/images/pay for courses-img1.png" class="jlImg"></image>
				<view class="flex-1 pl-2">
					<view class="pb-3"><text class="font-30 pr-2 font-w">张思雨</text> 15684792384</view>
					<view class="font-24 color6">
						<view>中国健美先回CCBA专业健身教练认证</view>
						<view>中国健美先回CCBA专业健身教练认证</view>
						<view>中国健美先回CCBA专业健身教练认证</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view>
			<view class="font-30 flex nav">
				<view class="tt" :class="navCurr==0?'on':''" @click="changeNav(0)">课程简介</view>
				<view class="tt" :class="navCurr==1?'on':''" @click="changeNav(1)">用户评价</view>
			</view>
			
			<view v-show="navCurr==0" class="p-r pt-5 navCon px-2">
				<view class="pb-5">
					<view class="flex p-r tit">
						<image src="../../static/images/pay-courses-icon5.png" class="numImg"></image>
						<view class="font-40 colorf p-r px-4">01</view>
						<view class="pl-1">课程内容</view>
					</view>
					<view class="font-26 pt-3">
						<view>
							人五河士道室内装修三级导航珠江新城世纪新城能解决可视电话三级导航收单据号是抠脚大汉卡是大货车可视对讲上课时间是拉KDJ可视对讲了卡SJD可视对讲卡萨丁就
						</view>
						<image src="../../static/images/pay for courses-img2.png" class="conImg"></image>
					</view>
				</view>
				
				<view class="pb-5">
					<view class="flex p-r tit">
						<image src="../../static/images/pay-courses-icon5.png" class="numImg"></image>
						<view class="font-40 colorf p-r px-4">02</view>
						<view class="pl-1">课程内容</view>
					</view>
					<view class="font-26 pt-3">
						<view>
							人五河士道室内装修三级导航珠江新城世纪新城能解决可视电话三级导航收单据号是抠脚大汉卡是大货车可视对讲上课时间是拉KDJ可视对讲了卡SJD可视对讲卡萨丁就
						</view>
					</view>
				</view>
				
				<view class="pb-5">
					<view class="flex p-r tit">
						<image src="../../static/images/pay-courses-icon5.png" class="numImg"></image>
						<view class="font-40 colorf p-r px-4">03</view>
						<view class="pl-1">约课流程</view>
					</view>
					<view class="font-24 flex1 pt-3">
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
				
			</view>
			
			<view v-show="navCurr==1" class="p-r pt-2 navCon px-2">
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
		</view>
		
		
		<view style="height: 200rpx;"></view>
		<view class="bg-white p-f left-0 right-0 bottom-0 flex1 p-2 bT-e1">
			<view class="font-26">已预约0/5人，还差<text class="colorR">3</text>人开课</view>
			<view class="criBtn" @click="Router.navigateTo({route:{path:'/pages/leagueClass-order/leagueClass-order'}})">立即预约</view>
		</view>
		
		
		
		<!-- 不是会员弹框 -->
		<view class="bg-mask" v-show="is_show">
			<view class="noVip bg-white radius10 text-center line-h m-a">
				<view class="py-2">
					<view class="pt-5 pb-4">很抱歉，您还不是怪力牛会员</view>
					<view class="pb-5 color9 font-24">成为会员，团课免费预约</view>
				</view>
				<view class="flex bT-f5">
					<view class="py-4 w-50 bR-f5" @click="isShow">我知道了</view>
					<view class="py-4 w-50 colorM">去看看</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				navCurr:0,
				is_show:false
			}
		},
		methods: {
			changeNav(i){
				const self = this;
				self.navCurr = i
			},
			isShow(){
				const self = this;
				self.is_show = !self.is_show
			}
		}
	}
</script>

<style scoped>
.banner{width: 100%;height: 580rpx;}
.content{margin-top: -50rpx;}
.rq-icon{width: 32rpx;height: 28rpx;}
.jlImg{width: 180rpx;height: 220rpx;border-radius: 10rpx;}
.tt{width: 25%;padding: 40rpx 20rpx 20rpx;}
.nav .on{font-weight: 800;position: relative;}
.nav .on::before{content: '';width: 40rpx;height: 6rpx;position: absolute;left: 50%;bottom: -6rpx;background-color: #FF633A;margin-left: -40rpx;}

.numImg{width: 106rpx;height: 60rpx;position: absolute;bottom: 0;left: 10rpx;}
.navCon .tit{background-color: #FFEBE6;line-height: 50rpx;}
.navCon .conImg{height: 320rpx;margin-top: 30rpx;}
.LC image{width: 52rpx;height: 48rpx;margin-bottom: 30rpx;}
.lcIcon5{width: 40rpx;height: 44rpx;margin-bottom: 30rpx;}
.GZ::before{content: "";border-left: 1px solid #e1e1e1;position: absolute;left: 0;height: 60rpx;}

.criBtn{line-height: 80rpx;border-radius: 40rpx;margin: 0;width: 260rpx;}

.noVip{width: 620rpx;margin-top: 65%;}
</style>
