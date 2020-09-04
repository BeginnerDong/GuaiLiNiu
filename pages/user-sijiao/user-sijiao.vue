<template>
	<view>

		<view class="p-r z10 bg-white p-s mb-2 top-0 shadowM">
			<view class="flex nav">
				<view class="li" :class="navCurr==0?'on':''" @click="changeNav(0)">全部</view>
				<view class="li" :class="navCurr==1?'on':''" @click="changeNav(1)">待使用</view>
				<view class="li" :class="navCurr==2?'on':''" @click="changeNav(2)">待评价</view>
				<view class="li" :class="navCurr==3?'on':''" @click="changeNav(3)">已评价</view>
			</view>
		</view>
		<block v-for="(item,index) in mainData" :key="index">
			<view class="shadowM bg-white px-2 mx-2 mb-2 radius10 line-h">
				<view class="font-24 color6 flex1 py-3 bB-f5">
					<view>订单编号：{{item.order_no}}</view>
					<view class="colorR" v-show="item.transport_status==0">待使用</view>
					<view class="colorR" v-show="item.transport_status==1">使用中</view>
					<view class="colorR" v-show="item.transport_status==2&&item.isremark==0">待评价</view>
					<view class="colorR" v-show="item.transport_status==2&&item.isremark==1">已评价</view>
				</view>
				<view class="flex1 py-3 bB-f5 w-100">
					<image :src="item.product[0].mainImg[0].url" class="wh180 radius10"></image>
					<view class="px-2 py-1 flex-1 d-flex flex-column j-sb h-180 w490">
						<view class="font-30 font-w">{{item.product[0].title}}
							<text class="colorR font-28 pl-2">(共{{item.standard}}节)</text>
						</view>
						<view class="flex">
							<block v-for="(c_item,c_index) in item.product[0].description" :key="c_index">
								<view class="tag">{{c_item}}</view>
							</block>
						</view>
						<view class="colorR">{{item.coach.name}} |<text class="price"> {{item.product[0].price}}</text>/<!-- {{item.product[0].score}} -->课时</view>
					</view>
				</view>
				<view class="font-26 color6 py-3 bB-f5 flex1" v-show="item.transport_status==1">
					<!-- <view>课程有效期：{{item.product[0].duration}}天</view> -->
					<view>已使用{{item.orderLog.length}}节，剩余<text class="colorR">{{item.standard-item.orderLog.length}}</text>节</view>
				</view>
				<!-- 其他 -->
				<view class="d-flex j-end bB-f5">
					<view class="btn b-e1 my-3" @click="goNext('use',item)" v-show="item.transport_status==0||item.transport_status==1">立即使用</view>
					<view class="btn b-e1 my-3" @click="goNext('comment',item)" v-show="item.transport_status==2&&item.isremark==0">立即评价</view>
					<view class="btn b-e1 my-3" @click="goNext('checkComment',item)" v-show="item.transport_status==2&&item.isremark==1">查看评价</view>
				</view>

				<!-- 进行中 -->
				<view class="py-3 flex1 bB-f5" v-show="item.transport_status==1"
				v-for="(c_item,c_index) in item.orderLog" :key="c_index">
					<image :src="c_item.qrcode" class="wh80" 
					@click="bigImg(c_item.qrcode)" v-show="c_item.qrcode&&c_item.is_use!=1"></image>
					<text class="flex-1 pl-1 line-h-md">预约时间：
						{{c_item.book_time_change+' '}}
						<text class="colorM">{{c_item.time}}</text> 
						{{c_item.is_use==1?'已结束':''}}
					</text>
					<view class="btn b-e1" @click="remove(c_item.id)" >取消预约</view>
				</view>
				
			</view>
		</block>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				navCurr: 0,
				mainData: [],
				searchItem: {
					thirdapp_id: 2,
					course_type: 3,
					pay_status:1
				},
				isLoadAll: false,
				time:'',
				page:0
			}
		},

		onLoad(options) {
			const self = this;
			self.page = 0;
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
		onShow() {
			const self = this;
			console.log('参数',self.page)
			if(self.page == 1){
				self.getMainData(true)
			}
		},
		methods: {
			
			bigImg(url){
				let _this = this;
				let imgsArray = [];
				imgsArray[0] = url
				uni.previewImage({
					current: 0,
					urls: imgsArray
				});
			},

			goNext(type, item) {
				const self = this;
				uni.setStorageSync('orderDetail', item);
				if (type == "comment") {
					self.Router.navigateTo({
						route: {
							path: '/pages/user-comment/user-comment?type=0'
						}
					});
				} else if (type == "use") {
					self.Router.navigateTo({
						route: {
							path: '/pages/user-sijiaoTime/user-sijiaoTime'
						}
					});
				} else if (type == "checkComment") {
					self.Router.navigateTo({
						route: {
							path: '/pages/user-commentDetail/user-commentDetail?type=0'
						}
					})
				}
			},
			
			remove(id){
				const self = this;
				
			},


			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					product: {
						tableName: 'Product',
						middleKey: 'product_id',
						key: 'id',
						searchItem: {
							status: 1
						},
						condition: '=',
					},
					orderLog: {
						tableName: 'OrderLog',
						middleKey: 'order_no',
						key: 'order_no',
						searchItem: {
							status: 1
						},
						condition: '=',
					},
					coach: {
						tableName: 'Coach',
						middleKey: 'coach_no',
						key: 'user_no',
						searchItem: {
							status: 1
						},
						condition: '=',
						info: ['name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.isLoadAll = true;
					};
					for (var i = 0; i < self.mainData.length; i++) {
						self.mainData[i].product[0].description = self.mainData[i].product[0].description.split(',');
						const timeArr = self.mainData[i].product[0].book_time_item.split(',');
						for(var j=0;j<self.mainData[i].orderLog.length;j++){
							const book_time = '';
							book_time = self.$Utils.timeto(parseInt(self.mainData[i].orderLog[j]['book_time'])*1000,'ymd-hm');
							for(var k=0; k<timeArr.length; k++){
								if(book_time.split(' ')[1]==timeArr[k].split('-')[0]){
									self.mainData[i].orderLog[j]['time'] = timeArr[k]
								}
							}
							self.mainData[i].orderLog[j]['book_time_change'] = book_time.split(' ')[0];
						}
					}
					uni.setStorageSync('canClick', true);
					// console.log('订单',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},

			changeNav(i) {
				const self = this;
				self.navCurr = i;
				switch (i) {
					case 0:
						self.searchItem = {
							thirdapp_id: 2,
							course_type: 3,
							pay_status:1
						};
						break;
					case 1:
						self.searchItem = {
							thirdapp_id: 2,
							course_type: 3,
							pay_status:1,
							transport_status: 0
						};
						break;
					case 2:
						self.searchItem = {
							thirdapp_id: 2,
							course_type: 3,
							pay_status:1,
							transport_status: 2,
							isremark: 0
						};
						break;
					case 3:
						self.searchItem = {
							thirdapp_id: 2,
							course_type: 3,
							pay_status:1,
							transport_status: 2,
							isremark: 1
						};
						break;
				};
				self.getMainData(true);
			}

		}
	}
</script>
<style>
	page {
		background-color: #f5f5f5;
	}
</style>
<style scoped>
	.li {
		width: 25%;
	}

	.btn {
		width: 160rpx;
		line-height: 60rpx;
		text-align: center;
		border-radius: 5rpx;
		margin-left: 20rpx;
	}

	.h-180 {
		height: 180rpx;
	}
</style>
