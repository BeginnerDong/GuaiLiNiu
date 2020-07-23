<template>
	<view>
		
		<!-- banner -->
		<view class="banner m-2">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff">
				<swiper-item>
					<view class="swiper-item">
						<image src="../../static/images/home-banner.png" ></image>
					</view>
				</swiper-item>
			</swiper>
		</view>
		
		<!-- 金刚区 -->
		<view class="flex2 py-3">
			<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/leagueClass/leagueClass'}})">
				<image src="../../static/images/home-icon.png" class="home-icon"></image>
				<view>团课</view>
			</view>
			<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/payLeagueClass/payLeagueClass'}})">
				<image src="../../static/images/home-icon1.png" class="home-icon"></image>
				<view>付费团课</view>
			</view>
			<view class="flex4" @click="Router.navigateTo({route:{path:'/pages/sijiao/sijiao'}})">
				<image src="../../static/images/home-icon2.png" class="home-icon"></image>
				<view>私教</view>
			</view>
		</view>
		
		<!-- 门店列表 -->
		<view class="px-2 p-r">
			<image src="../../static/images/home-img.png" class="homeBg"></image>
			
			<view class="p-r line-h px-3">
				<view class="d-flex a-start j-sb">
					<view class="font-30 font-w py-3">门店列表</view>
					<view class="font-22 p-1 b-e1 radius" @click="Router.navigateTo({route:{path:'/pages/store/store'}})">逛逛其他店</view>
				</view>
				<view @click="Router.navigateTo({route:{path:'/pages/storeDetail/storeDetail'}})">
					<view class="flex py-3">
						<view class="font-34">西安外事学院</view>
						<view class="font-20 colorf p-r sq">
							<image src="../../static/images/home-icon5.png" class="sq-icon"></image>
							<view class="top-0 p-a t-indent10 line-h-md">社区店</view>
						</view>
					</view>
					<view class="flex1">
						<image src="../../static/images/home-icon3.png" class="homeDz-icon"></image>
						<view class="flex-1 flex">
							<view class="font-24 pr-2">西安市雁塔区鱼斗路左岸春天A座2025</view>
							<view class="font-20 bg-f5 d-inline-block line-h-md px-1">2.21KM</view>
						</view>
						<image src="../../static/images/home-icon4.png" class="R-icon"></image>
					</view>
				</view>
				
				<view class="pt-3">
					<view class="font-30 font-w py-3">门店活动</view>
					<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff" class="banner1">
						<swiper-item>
							<view class="swiper-item" @click="Router.navigateTo({route:{path:'/pages/limitedTime/limitedTime'}})">
								<image src="../../static/images/home-img1.png" ></image>
							</view>
						</swiper-item>
					</swiper>
				</view>
				
				<view class="pt-2">
					<view class="font-30 py-3 flex1">
						<view class="font-w">门店课程</view>
						<view class="flex" @click="Router.navigateTo({route:{path:'/pages/sijiao-course/sijiao-course'}})">
							<view class="font-26 color9 pr-2">全部</view>
							<image src="../../static/images/the order-icon3.png" class="R-icon"></image>
						</view>
					</view>
					<view class="line-h flexX py-1">
						<view class="shadowM radius10 flex-shrink mr-3 p-r">
							<image src="../../static/images/sijiao-img.png" class="kcImg"></image>
							<view class="p-3">
								<view class="font-30 font-w">减脂训练营·中上</view>
								<view class="font-24 pt-3">Auger | 20:00~20:45</view>
							</view>
							<view class="font-20 colorf kcSgin">差一个人开课</view>
						</view>
					</view>
				</view>
				
				<view class="pt-2 pb-3">
					<view class="font-30 py-3 flex1">
						<view class="font-w">推荐私教</view>
						<view class="flex" @click="Router.navigateTo({route:{path:'/pages/store-sijiao/store-sijiao'}})">
							<view class="font-26 color9 pr-2">全部</view>
							<image src="../../static/images/the order-icon3.png" class="R-icon"></image>
						</view>
					</view>
					<view class="line-h">
						<view class="shadowM radius10 flex1 p-2 mb-3">
							<image src="../../static/images/home-img3.png" class="sjImg"></image>
							<view class="p-3 flex-1">
								<view class="flex">
									<view class="font-30 font-w pr-2">张思雨</view>
									<view class="flex">
										<image src="../../static/images/home-icon6.png" class="zs-icon"></image>
										<view class="colorZS font-24 pl-1">专业证书（3）</view>
									</view>
								</view>
								<view class="flex py-4">
									<view class="tag tagY">矫正</view>
									<view class="tag tagB">提升柔软度</view>
									<view class="tag tagG">改善身体线条</view>
								</view>
								<view class="flex">
									<view class="price font-26 pr-5">220/节</view>
									<view class="font-18 color9 pl-4">累计：105节</view>
								</view>
							</view>
						</view>
						<view class="shadowM radius10 flex1 p-2 mb-3">
							<image src="../../static/images/home-img3.png" class="sjImg"></image>
							<view class="p-3 flex-1">
								<view class="flex">
									<view class="font-30 font-w pr-2">张思雨</view>
									<view class="flex">
										<image src="../../static/images/home-icon6.png" class="zs-icon"></image>
										<view class="colorZS font-24 pl-1">专业证书（3）</view>
									</view>
								</view>
								<view class="flex py-4">
									<view class="tag tagY">矫正</view>
									<view class="tag tagB">提升柔软度</view>
									<view class="tag tagG">改善身体线条</view>
								</view>
								<view class="flex">
									<view class="price font-26 pr-5">220/节</view>
									<view class="font-18 color9 pl-4">累计：105节</view>
								</view>
							</view>
						</view>
					</view>
					<view class="criBtn">进入店铺</view>
				</view>
				
			</view>
		</view>
		
		
		<view class="homeBto mx-2 my-5 radius10 p-3 flex1 p-r" v-show="is_show" @click="Router.navigateTo({route:{path:'/pages/experienceCoupon/experienceCoupon'}})">
			<image src="../../static/images/home-img4.png" class="Img80"></image>
			<view class="pl-2 pr-5 flex-1">店长送你一份信任见面礼，如需到店使用请提前预约~</view>
			<view class="criBtn criBtn1">去领取</view>
			<image src="../../static/images/my class-icon1.png" class="x-icon1" @click="isShow"></image>
		</view>
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1-a.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar2.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: true,
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				
			},
			isShow(){
				const self = this;
				self.is_show = !self.is_show;
			}
		}
	};
</script>
<style scoped>
swiper{height: 250rpx;}
.banner{width: 710rpx;height: 250rpx;border0-radius:20rpx;overflow: hidden;}
.banner image{width: 100%;height: 250rpx;}
.home-icon{width: 112rpx;height: 112rpx;margin-bottom: 20rpx;}
.homeBg{width: 722rpx;height: 1818rpx;position: absolute;top: 0;left: 0;right: 0;margin: 0 auto;}
.sq{top: -10rpx;right: -10rpx;}
.banner1{width: 650rpx;height: 200rpx;}
.banner1 image{width: 650rpx;height: 200rpx;}
.kcImg{width: 420rpx;height: 230rpx;}

.sjImg{width: 160rpx;height: 196rpx;}
.homeBto{background-color: #3F2E2A;font-size: 24rpx;color: #fff;}
.criBtn1{width: 110rpx;line-height: 40rpx;border-radius: 20rpx;}

</style>
