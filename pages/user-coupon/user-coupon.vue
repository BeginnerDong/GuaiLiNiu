<template>
	<view>
		
		<view class="p-r z10 bg-white p-s top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">大厅</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">未使用</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">已使用</view>
				<view class="li" :class="navCurr==-1?'on':''" @click="changeNav(-1)">已过期</view>
			</view>
		</view>
		
		<view class="pb-3">
			<block v-for="(item,index) in mainData" :key="index">
				<view class="mt-3 colorf p-r" v-if="!item.snap_coupon" >
					<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
					<view class="p-aXY p-3 top-0 left-0 flex1">
						<view class="px-1 flex-1">
							<view class="font-40 pb-5">满{{item.condition}}元减{{item.value}}元</view>
							<view class="font-22">
								<view>有效期：{{item.valid_time}}天</view>
								
							</view>
						</view>
						<view class="font-30 colorM pr-5" @click="submit(item)">立即领取</view>
					</view>
				</view>
				
				<view class="mt-3 colorf p-r" v-if="item.snap_coupon&&item.use_step==navCurr" >
					<image src="../../static/images/coupons-icon.png" class="couponImg"></image>
					<view class="p-aXY p-3 top-0 left-0 flex1">
						<view class="px-1 flex-1">
							<view class="font-40 pb-5">满{{item.snap_coupon.condition}}元减{{item.snap_coupon.value}}元</view>
							<view class="font-22">
								<view>有效期至：{{item.invalid_time_change}}</view>
							</view>
						</view>
						<view class="flex4">
							<button  open-type='share'  :data-couponNo="item.snap_coupon.coupon_no" :data-parentNo="item.snap_coupon.user_no" class="font-30 colorM pr-5 pb-2" >分享</button>
							<view class="font-30 colorM pr-5"
							@click="Router.navigateTo({route:{path:'/pages/index/index'}})">立即使用</view>
						</view>
					</view>
					
					<!-- <button type="default" @click="share(item)">分享</button>
					<view  >分享</view> -->
				</view>
			</block>
			
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navCurr:0,
				mainData:[],
				searchItem:{
					thirdapp_id: 2
				},
				isLoadAll:false,
				Router:this.$Router
			}
		},
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		onReachBottom() {
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		methods: {
			
			share(item){
				const self = this;
				
				wx.requestSubscribeMessage({
					tmplIds: [
						'uEfkFlQDr6pZYDBUWvQOHzRhKioUK_F1BnohDxr-2i4'
					],
					success(res) {
						console.log('res', res)
					},
					fail(err){
						uni.getStorageSync({
							title:'拒绝后将不再接受分享的奖励提醒'
						})
					}
				});
				
				self.$Router.navigateTo({
					route:{path:'/pages/coupon/coupon?coupon_no='
					+item.snap_coupon.coupon_no
					+'&&parent_no='
					+item.user_no},
				});
			},
			
			
			onShareAppMessage: function( options ){
			　　var that = this;
				
				var path = '/pages/coupon/coupon?coupon_no='
					+options.target.dataset.couponno
					+'&&parent_no='
					+options.target.dataset.parentno;
				console.log('path',path);
			　　// 设置菜单中的转发按钮触发转发事件时的转发内容
			　　var shareObj = {
			　　　　title: "免费领取优惠卷",        // 默认是小程序的名称(可以写slogan等)
			　　　　path: path,        // 默认是当前页面，必须是以‘/’开头的完整路径
			　　　　imgUrl: '',     //自定义图片路径，可以是本地文件路径、代码包文件路径或者网络图片路径，支持PNG及JPG，不传入 imageUrl 则使用默认截图。显示图片长宽比是 5:4
			　　};
			　　// 来自页面内的按钮的转发
			/* 　　if( options.from == 'button' ){
			　　　　var eData = options.target.dataset;
			　　　　console.log( eData.name );     // shareBtn
			　　　　// 此处可以修改 shareObj 中的内容
			　　　　shareObj.path = '/pages/btnname/btnname?btn_name='+eData.name;
			　　} */
			　　// 返回shareObj
			　　return shareObj;
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				const postData = {};
				if(self.navCurr>0){
					postData.tokenFuncName = 'getProjectToken';
				};
				
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					'create_time':'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							if(self.mainData[i].invalid_time){
								self.mainData[i].invalid_time_change = self.$Utils.timeto(parseInt(self.mainData[i].invalid_time),'ymd-hm');	
							};
							
							// self.mainData[i].description = self.mainData[i].description.split(',');
							// self.mainData[i].start_time = self.$Utils.timeto(parseInt(self.mainData[i].start_time),'hm')
							self.mainData[i].end_time = self.$Utils.timeto(parseInt(self.mainData[i].end_time),'hm')
						};
					}else{
						self.isLoadAll = true;
					};
					uni.setStorageSync('canClick', true);
					console.log('coupon',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				if(self.navCurr>0){
					self.$apis.userCouponGet(postData, callback);
				}else{
					self.$apis.couponGet(postData, callback);
				}
				
			},
			
			changeNav(i){
				const self = this;
				if(self.navCurr!=i&&i!=0&&self.navCurr!=0){
					self.navCurr = i;
				}else{
					self.navCurr = i;
					self.getMainData(true)
				};
			},
			
			submit(item){
				const self = this;
				self.chooseCouponData = item;
				uni.setStorageSync('canClick', false);
				var couponList = []
				
				couponList.push({coupon_id:self.chooseCouponData.id,
					count:1,
					type:1,
					data:{
						course_type:self.chooseCouponData.course_type
					}
				});
				self.addOrder(couponList)
			},
			
			addOrder(couponList) {
				const self = this;	
				const postData = {}; 
				postData.couponList = self.$Utils.cloneForm(couponList);
				postData.tokenFuncName = 'getProjectToken';
				console.log('addOrder',postData);
				const callback = (res) => {
					
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
						uni.setStorageSync('canClick', true);
					};		
				};
				self.$apis.addCoupon(postData, callback);
			},
			
			goPay() {
				const self = this;
				const postData = {};
				console.log('self.chooseCouponData.price',self.chooseCouponData.price)
				postData.otherPay={
					price:parseFloat(self.chooseCouponData.price)
				};
				
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				
				
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '领取成功',
										duration: 1000,
										success: function() {
											
										}
									});
									self.changeNav(1);
									/* setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user/user'}})
									}, 1000); */
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '领取成功',
								duration: 1000,
								success: function() {
									
								}
							});
							self.changeNav(1);
							// setTimeout(function() {
							// 	self.$Router.redirectTo({route:{path:'/pages/user/user'}})
							// }, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
						self.changeNav(1);
					};
					uni.setStorageSync('canClick', true);
				};
				self.$apis.couponPay(postData, callback);
			},
		}
	}
</script>
<style>
page{background-color: #f5f5f5;}
</style>
<style scoped>
.li{width: 25%;}
.couponImg{width: 713rpx;height: 220rpx;margin: auto;}
.lq{width: 160rpx;line-height: 50rpx;border-radius: 10rpx;text-align: center;margin-right: 40rpx;}
button{background: rgba(0,0,0,0);}
</style>
