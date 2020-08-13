<template>
	<view>
		
		<view class="p-r">
			<image src="../../static/images/about-img.png" class="userBg"></image>
			<view class="p-aXY" :style="{marginTop:statusBar+'px'}">
				<view class="head">我的</view>
				<view class="colorf px-2 py-3 flex">
					<view class="wh130 radius-5 overflow-h">
						<open-data type="userAvatarUrl"></open-data>  
					</view>
					
					<view class="pl-2">
						<view class="font-32"><open-data type="userNickName" lang="zh_CN"></open-data> </view>
						<view class="userSgin my-1">{{userData.info.behavior==1?'会员':'普通用户'}}</view>
						<view v-if="userData.info.deadline>0" >有效期：{{userData.info.deadline_change}}</view>
					</view>
				</view>
				<view class="px-2 Mgb p-r">
					<image src="../../static/images/members-img1.png" class="vipBg"></image>
					<view class="p-aX top-0 px-5 py-3 flex1">
						<view>
							<view class="flex font-30 font-w">
								<image src="../../static/images/members-icon2.png" class="vip-icon"></image>
								<view>VIP会员</view>
								<!-- <view>会员码：2658475</view> -->
							</view>
							<view class="pt-4 font-24">享受更多的优惠权益</view>
							<!-- <view class="pt-4 font-24">有效期：2020.06.23-2021.06.23</view> -->
						</view>
						<view class="criBtn" @click="Router.navigateTo({route:{path:'/pages/VIP/VIP?vip=0'}})">立即开通</view>
						<!-- <view class="criBtn" @click="Router.navigateTo({route:{path:'/pages/VIP/VIP?vip=1'}})">立即续费</view> -->
					</view>
				</view>
			</view>
		</view>
		
		<view class="bg-white p-r px-2 line-h" :class="statusBar==20?'userCon':''">
			<view class="font-34 font-w pt-5">我的课程</view>
			<view class="flex py-3 bB-f5 font-24">
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/user-leagueClass/user-leagueClass'}})">
					<image src="../../static/images/about-icon.png" class="userIcon1"></image>
					<view>团课</view>
				</view>
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/user-sijiao/user-sijiao'}})">
					<image src="../../static/images/about-icon1.png" class="userIcon1"></image>
					<view>私教</view>
				</view>
			</view>
			
			<view class="font-34 font-w pt-5">我的服务</view>
			<view class="flex py-4 font-24">
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/user-coupon/user-coupon'}})">
					<image src="../../static/images/about-icon2.png" class="userIcon2"></image>
					<view>优惠券</view>
				</view>
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/aboutUS/aboutUS'}})">
					<image src="../../static/images/about-icon3.png" class="userIcon3"></image>
					<view>关于我们</view>
				</view>
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/joinUS/joinUS'}})">
					<image src="../../static/images/about-icon5.png" class="userIcon4"></image>
					<view>加盟我们</view>
				</view>
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/face/face'}})">
					<image src="../../static/images/about-icon4.png" class="userIcon5"></image>
					<view>人脸录入</view>
				</view>
				<view class="flex4 itt" @click="Router.navigateTo({route:{path:'/pages/user-sijiaoEntrance/user-sijiaoEntrance'}})">
					<image src="../../static/images/about-icon6.png" class="userIcon6"></image>
					<view>教练入口</view>
				</view>
			</view>
		</view>
		
		<view class="pl-2 font-24 colorf p-r mt-5">
			<image src="../../static/images/about-img2.png" class="userImg"></image>
			<view class="p-aXY txt">健身房是现代剧好几家肯德基六款新车承接数控刀具拉可视对讲</view>
		</view>
		
		
		<view style="height: 130rpx;"></view>
		<!-- footer -->
		<view class="footer">
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<image src="../../static/images/nabar1.png" mode=""></image>
				<view>首页</view>
			</view>
			<view class="item on" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
				<image src="../../static/images/nabar2-a.png" mode=""></image>
				<view>我的</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				Router:this.$Router,
				statusBar: app.globalData.statusBar,
				userData:{}
			}
		},
		onLoad(){
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
		},
		methods: {
			getUserData() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getProjectToken';
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						self.userData.info.deadline_change = self.$Utils.timeto(parseInt(self.userData.info.deadline*1000),'ymd-hm')
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
		}
	}
</script>

<style scoped>
.head{font-size: 36rpx;line-height: 100rpx;color: #fff;text-align: center;}
.userBg{height: 524rpx;}
.userSgin{line-height: 46rpx;width: 135rpx;font-size: 24rpx;color: #fff;text-align: center;border-radius: 23rpx;background-color: #FF8B6C;}
.criBtn{width: 160rpx;background-color: #E5C37B;color: #222;margin: 0;}

.itt{width: 20%;}
.userCon{margin-top: -30rpx;}
.userIcon1{width: 55rpx;height: 46rpx;margin-bottom: 30rpx;}
.userIcon2{width: 50rpx;height: 42rpx;margin-bottom: 30rpx;}
.userIcon3{width: 48rpx;height: 43rpx;margin-bottom: 30rpx;}
.userIcon4{width: 48rpx;height: 48rpx;margin-bottom: 30rpx;}
.userIcon5{width: 46rpx;height: 46rpx;margin-bottom: 30rpx;}
.userIcon6{width: 48rpx;height: 46rpx;margin-bottom: 30rpx;}

.userImg{height: 110rpx;width: 710rpx;}
.txt{padding: 15rpx 76rpx 15rpx 200rpx;}
</style>
