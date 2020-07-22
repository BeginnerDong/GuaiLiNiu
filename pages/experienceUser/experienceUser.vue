<template>
	<view>
		
		<view class="vipHead colorf font-24 line-h px-2">
			<view class="flex py-3 font-30 font-w" @click="Show()">
				<view>西安外事学院店</view>
			</view>
			<view>距离 2.22KM</view>
			<view class="flex1 py-3">
				<view>西安市雁塔区鱼斗路左岸春天A座2025</view>
				<image src="../../static/images/members-icon1.png" class="homeDz-icon"></image>
			</view>
			
			<view class="p-r color2">
				<image src="../../static/images/members-img1.png" class="vipBg"></image>
				<view class="p-aX top-0 px-5 py-3">
					<view class="flex font-30 font-w">
						<image src="../../static/images/members-icon2.png" class="vip-icon"></image>
						<view>VIP会员</view>
					</view>
					<view class="pt-4 font-24">有效期：2020.06.23-2021.06.23</view>
				</view>
			</view>
		</view>
		
		<view class="p-r line-h">
			<image src="../../static/images/members-img2.png" class="vipBg1 m100"></image>
			
			<view class="p-aXY ">
				
				<view class="pt-3">
					<view class="font-w py-4 px-2">会员权益</view>
					<view class="flex mb-4 flex-wrap font-24 vipBox">
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon4.png" class="memberIcon"></image>
							<view>器械不限用</view>
						</view>
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon5.png" class="memberIcon"></image>
							<view>到店体验服务</view>
						</view>
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon6.png" class="memberIcon"></image>
							<view>团课免费约</view>
						</view>
						<view class="flex4 vipItem">
							<image src="../../static/images/members-icon7.png" class="memberIcon"></image>
							<view>7*24营业</view>
						</view>
					</view>
				</view>
				
			</view>
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tcCurr:0,
				vip:1
			}
		},
		onLoad(option){
			const self = this;
			self.vip = option.vip
		},
		methods: {
			changeTc(i){
				const self = this;
				self.tcCurr = i;
			}
		}
	}
</script>

<style>
.vipHead{background: linear-gradient(to bottom, #A47B4C 20%,#B3916B 50%);}
.vipBg1{height: 859rpx;}
.m100{margin-top: -100rpx;}
.tc{width: 220rpx;height: 140rpx;text-align: center;line-height: 1;padding-top: 30rpx;}
.tcBox .on{background-color: #FFF5F2;border: 1px solid #FF633A;}
.yes{width: 61rpx;height: 51rpx;position: absolute;bottom: 0;right: 0;}

.memberIcon{width: 48rpx;height: 53rpx;margin-bottom: 30rpx;}
.vipItem{width: 25%;margin-bottom: 30rpx;}
</style>
