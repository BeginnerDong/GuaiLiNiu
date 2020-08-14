<template>
	<view>
		
		<view class="vipHead colorf font-24 line-h px-2">
			<view class="flex py-3 font-30 font-w">
				<view>{{shopData.name}}</view>
				<image src="../../static/images/members-icon.png" class="sj-icon"></image>
			</view>
			<view>距离 {{shopData.distance}}KM</view>
			<view class="flex1 py-3">
				<view>{{shopData.address}}</view>
				<image src="../../static/images/members-icon1.png" class="homeDz-icon"></image>
			</view>
			
			<view class="p-r color2">
				<image src="../../static/images/members-img1.png" class="vipBg"></image>
				<view class="p-aX top-0 px-5 py-3">
					<view class="flex font-30 font-w">
						<image src="../../static/images/members-icon2.png" class="vip-icon"></image>
						<view>{{userData.info.behavior==0?'VIP会员':'会员'}}</view>
					</view>
					<view class="pt-4 font-24">{{userData.info.behavior==0?'享受更多的优惠权益':'到期时间：'+userData.info.deadline_change}}</view>
				</view>
			</view>
		</view>
		
		<view class="p-r line-h">
			<image src="../../static/images/members-img2.png" class="vipBg1 m100"></image>
			
			<view class="p-aXY ">
				<view>
					<view class="mt-2 px-2">
						<view class="font-w py-4">选择套餐</view>
						<view class="flex1 mb-4 tcBox">
							<view class="tc b-e1 p-r radius10" 
							v-for="(item,index) in mainData" :key="index"
							:class="tcCurr==index?'on':''" @click="changeTc(index,item)">
								<view class="font-24 pb-3">{{item.title}}</view>
								<view class="price font-32">{{item.price}}</view>
								<image src="../../static/images/members-icon3.png" class="yes" v-show="tcCurr==index"></image>
							</view>
						</view>
					</view>
					<view class="f5Bj-H20"></view>
				</view>
				
				<view class="">
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
					
					<button class="btnAuto" 
					v-show="userData.info.behavior==1"
					open-type="getUserInfo"
					@getuserinfo="Router.navigateTo({route:{
						path:'/pages/VIP-information/VIP-information?product_id='
						+product_id}})">
						<text class="price">{{price}}</text>/{{mainData[tcCurr].title}} 立即续费
					</button>
				</view>
			</view>
		</view>
		
		<view v-show="userData.info.behavior==0">
			<view style="height: 130rpx;"></view>
			<view class="bg-white p-f left-0 right-0 bottom-0 flex1 carBot pl-3 bT-e1">
				<view class="font-24"><text class="price font-w font-36">{{price}}</text>/{{duration}}天</view>
				<!-- <button class="carBtn" size="mini" open-type="getUserInfo" 
				@getuserinfo="Router.navigateTo({route:{
					path:'/pages/VIP-information/VIP-information?product_id='
					+product_id
					+'&&price='
					+price
					+'&&duration='
					+duration}})
					">确认支付</button> -->
				<view class="carBtn" @click="Router.navigateTo({route:{path:'/pages/VIP-information/VIP-information?product_id='+product_id}})">确认支付</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tcCurr:0,
				mainData:[],
				product_id:0,
				price:0,
				duration:0,
				searchItem:{
					thirdapp_id: 2,
					type: 2,
					user_type: 1
				},
				Router:this.$Router,
				shopData:{},
				userData:{}
			}
		},
		onLoad(option){
			const self = this;
			self.shopData = uni.getStorageSync('shopData');
			self.userData = uni.getStorageSync('user_info');
			self.userData.info.deadline_change = self.$Utils.timeto(parseInt(self.userData.info.deadline*1000),'ymd-hm')
			self.searchItem.shop_no = uni.getStorageSync('shopData')['user_no'];
			self.$Utils.loadAll(['getMainData'], self);
			
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						self.product_id = self.mainData[self.tcCurr].id;
						self.price = self.mainData[self.tcCurr].price;
						self.duration = self.mainData[self.tcCurr].duration;
					};
					console.log(self.mainData)
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			changeTc(i,item){
				const self = this;
				self.tcCurr = i;
				self.product_id = item.id;
				self.price = item.price;
				self.duration = item.duration;
			},
			
			
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

.btnAuto{width: 710rpx;margin-top: 200rpx;}
.btnAuto .price{color: #fff;}

.carBtn{height: 100rpx;border-radius: 0;line-height: 100rpx!important;}
</style>
