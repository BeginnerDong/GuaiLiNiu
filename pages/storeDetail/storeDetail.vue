<template>
	<view :style="sb_show?'height:100%;overflow:hidden':''">
		
		<!-- banner -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" indicator-active-color="#fff" class="banner">
			<swiper-item>
				<image src="../../static/images/stores details-img.png" ></image>
			</swiper-item>
		</swiper>
		
		<view class="content bg-white radius20-T p-r px-2">
			<view class="pb-4 bB-f5">
				<view class="flex pt-5">
					<view class="font-34">西安外事学院</view>
					<view class="font-20 colorf p-r sq">
						<image src="../../static/images/home-icon5.png" class="sq-icon"></image>
						<view class="top-0 p-a t-indent10 line-h-md">社区店</view>
					</view>
				</view>
				<view class="shareSgin">
					<image src="../../static/images/course-icon2.png" class="fx-icon"></image>
					<view>分享</view>
				</view>
				<view class="font-22 line-h py-2 color9">
					门店面积 <text class="priceM pl-1">280</text> | 私教面积 <text class="priceM pl-1">50</text>
				</view>
				<view class="flex1">
					<view class="dz avoidOverflow2 font-26 color6">地址：西安市未央区谭家街道永城路230号万科幸福里10号楼1单元202</view>
					<view class="flex4 font-20 color6">
						<image src="../../static/images/stores details-icon.png" class="storeDz-icon mb-2"></image>
						<view>距离13.2KM</view>
					</view>
				</view>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="font-w t-indent20 line-h pt-4 pb-3 tit">限时特惠</view>
				<image src="../../static/images/stores details-img3.png" class="storeImg1" @click="Router.navigateTo({route:{path:'/pages/limitedTime/limitedTime'}})"></image>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="flex1">
					<view class="font-w t-indent20 line-h pt-4 pb-3 tit">门店详情</view>
					<view class="color6 flex1" @click="isShow(2)">
						<view>查看全部</view>
						<image src="../../static/images/the order-icon3.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="flex1 color6 font-24">
					<view class="flex4">
						<image src="../../static/images/stores details-icon2.png" class="wh80 mb-2"></image>
						<view>无线网络</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores details-icon3.png" class="wh80 mb-2"></image>
						<view>储物柜</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores details-icon4.png" class="wh80 mb-2"></image>
						<view>更衣室</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores details-icon5.png" class="wh80 mb-2"></image>
						<view>饮水机</view>
					</view>
					<view class="flex4">
						<image src="../../static/images/stores details-icon6.png" class="wh80 mb-2"></image>
						<view>人脸识别</view>
					</view>
				</view>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="flex1">
					<view class="font-w t-indent20 line-h pt-4 pb-3 tit">门店私教</view>
					<view class="color6 flex1" @click="Router.navigateTo({route:{path:'/pages/store-sijiao/store-sijiao'}})">
						<view>查看全部</view>
						<image src="../../static/images/the order-icon3.png" class="R-icon ml-1"></image>
					</view>
				</view>
				<view class="flexX line-h">
					<view class="flex-shrink p-r mr-3 overflow-h">
						<image src="../../static/images/sijiao-img.png" class="storeImg2"></image>
						<view class="font-w py-2">张瑞生</view>
						<view class="flex1">
							<view class="font-22 color6">极速瘦身、减脂塑形</view>
							<view class="font-20 color9">累计 105节</view>
						</view>
						<view class="tjSgin">店长推荐</view>
					</view>
				</view>
			</view>
			
			<view class="pb-4 bB-f5">
				<view class="font-w t-indent20 line-h pt-4 pb-3 tit">门店课程</view>
				<view class="font-26 text-center flex1 p-2 bB-f5 time">
					<view class="wh80" v-for="(v,i) in 6" :class="timeCurr==i?'on':''" @click="changeTime(i)">
						<view class="pt-1">今天</view>
						<view class="font-20 color6">6.24</view>
					</view>
				</view>
				<view class="shadow radius20 m-a p-r mb-3 tkBox">
					<image src="../../static/images/sijiao-img.png" class="kcImg"></image>
					<view class="px-2 py-3">
						<view class="font-30 flex1">
							<view class="font-w">减脂训练营·中上</view>
							<view><text class="price font-w">220</text>/9课时</view>
						</view>
						<view class="font-24 py-2">Auger | 20:00~20:45</view>
						<view class="flex">
							<view class="tag tagY">矫正</view>
							<view class="tag tagB">提升柔软度</view>
							<view class="tag tagG">改善身体线条</view>
						</view>
					</view>
					<view class="font-20 colorf kcSgin">差一个人开课</view>
				</view>
			</view>
			
		</view>
		
		<!-- 客服 -->
		<image src="../../static/images/stores details-icon1.png" class="kfImg" @click="isShow(1)"></image>
		<!-- 客服弹窗 -->
		<view class="bg-mask" v-show="kf_show">
			<view class="p-r kf">
				<view class="bg-white w-600 radius20 pb-5 font-24 p-r kfCon">
					<image src="../../static/images/stores details-img4.png" class="wh120 kfLogo"></image>
					<view class="font-30 text-center py-2">店长：大白</view>
					<view class="mx-3 mb-2 px-3 py-2 flex kfBg radius10">
						<view>联系方式：</view>
						<image src="../../static/images/stores details-icon18.png" class="wh50 mx-3"></image>
						<image src="../../static/images/stores details-icon19.png" class="wh50 mx-3"></image>
					</view>
					<view class="mx-3 mb-5 px-3 py-2 kfBg radius10 line-h-lg">
						<view>联系方式：<br/>欢迎来到怪力牛运动外事学院店，本店24小时营业，疫情期间请做好个人防护，如有问题请联系微店长  15562362356（微信同号）</view>
					</view>
				</view>
				<image src="../../static/images/stores details-icon20.png" class="wh30 m-4 xx" @click="isShow(1)"></image>
			</view>
		</view>
		
		<!-- 门店详情全部 -->
		<view class="bg-mask" v-show="sb_show">
			<view class="bg-white radius30-T p-a bottom-0 storeAll">
				<view class="p-r flexY h-100">
					<view class="font-34 pt-4 text-center">服务设备</view>
					
					<view class="py-3 font-30 font-w px-4">器械设备</view>
					<view class="px-5 flex flex-wrap">
						<view class="flex4 font-24 color6 mb-3 bb" v-for="v in 7">
							<image src="../../static/images/stores details-icon10.png" class="wh100 mb-2"></image>
							<view>跑步机</view>
						</view>
					</view>
					<view class="py-3 font-30 font-w px-4">基础服务</view>
					<view class="px-5 flex flex-wrap">
						<view class="flex4 font-24 color6 mb-3 bb" v-for="v in 7">
							<image src="../../static/images/stores details-icon10.png" class="wh100 mb-2"></image>
							<view>跑步机</view>
						</view>
					</view>
				</view>
				<image src="../../static/images/stores details-icon21.png" class="wh30 p-a m-2 right-0 z-index100 top-0" @click="isShow(2)"></image>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				timeCurr:0,
				kf_show:false,
				sb_show:false
			}
		},
		methods: {
			changeTime(i){
				const self = this;
				self.timeCurr = i
			},
			isShow(type){
				const self = this;
				if(type==1){
					self.kf_show = !self.kf_show;
				}else if(type == 2){
					self.sb_show = !self.sb_show;
				}
				
			}
		}
	}
</script>

<style scoped>
.banner{width: 100%;height: 510rpx;}
.content{margin-top: -50rpx;}
.sq{top: -10rpx;right: -10rpx;}
.dz{width: 514rpx;}
.content .tit::before{content: '';width: 8rpx;height: 20rpx;background-color: #FF633A;position: absolute;left: 20rpx;}
.storeImg1{width: 100%;height: 200rpx;border-radius: 10rpx;}
.storeImg2{width: 370rpx;height: 200rpx;border-radius: 10rpx;}
.time .on{background-color: #FF633A;border-radius: 50%; color: #fff!important;}
.time .on view{color: #fff;}

.kfImg{width: 130rpx;height: 110rpx;position: fixed;top: 60%;right: 0;}
.kfLogo{position: absolute;left: 0;right: 0;margin: auto;top: -50rpx;}
.kf{margin: 40% 75rpx 0;}
.kfCon{padding-top: 70rpx;}
.kfBg{background-color: #FFF9F5;}
.xx{margin: 100rpx auto;}

.storeAll{height: 90%;width: 100%;}
.bb{margin-right: 85rpx;}
.bb:nth-child(4n){margin-right: 0;}
</style>
