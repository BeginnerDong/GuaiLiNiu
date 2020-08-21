<template>
	<view class="font-26 line-h">
		
		<view class="top text-center py-2 colorf">
			<view class="pb-1">距离活动结束：</view>
			<countdown-timer ref="countdown" :time="time">
			    <template v-slot="{day, hour, minute, second}">
			        <!-- 你的样式部分 Start -->
			        <text class="font-32">{{day}}天{{hour}}时{{minute}}分{{second}}秒</text>
			        <!-- 你的样式部分 End -->
			    </template>     
			</countdown-timer>
		</view>
		
		<image src="../../static/images/Vpreferential-img.png" class="limitImg"></image>
		<veiw class="d-flex a-start px-2 mb-5">
			<image src="../../static/images/preferential-icon.png" class="wh30 mr-2"></image>
			<view>
				<view class="font-30 font-w pb-3">{{mainData.shop&&mainData.shop[0]?mainData.shop[0].name:''}}</view>
				<view>活动介绍：{{mainData.description?mainData.description:''}}</view>
			</view>
		</veiw>
		
		<view class="bg-white radius10 mx-2 py-5 mb-3">
			<view class="flex j-center">
				<image src="../../static/images/preferential-icon1.png" class="gk-icon"></image>
				<view class="font-36 px-2">购卡优惠</view>
				<image src="../../static/images/preferential-icon2.png" class="gk-icon"></image>
			</view>
			<view class="font-24 color6 text-center py-3">会员卡仅限{{mainData.shop&&mainData.shop[0]?mainData.shop[0].name:''}}使用</view>
			<view class="flex1 px-3">
				<image src="../../static/images/preferential-img1.png" class="nkImg"></image>
				<view class="flex-1 pl-2">
					<view class="font-24 pb-4">{{mainData.title}}</view>
					<view class="colorR">{{mainData.card[0].price}}元<text class="price line-through font-22 pl-3">{{mainData.card[0].o_price}}</text></view>
				</view> 
				<view class="criBtn" @click="Router.navigateTo({route:{path:'/pages/limitedTime-may/limitedTime-may?card='+card+'&name='+mainData.title}})">立即购买</view>
			</view>
		</view>
		
		<view class="bg-white radius10 mx-2 py-5 mb-5">
			<view class="flex j-center">
				<image src="../../static/images/preferential-icon1.png" class="gk-icon"></image>
				<view class="font-36 px-2">活动规则</view>
				<image src="../../static/images/preferential-icon2.png" class="gk-icon"></image>
			</view>
			<view class="pt-3 px-3 color6">
				<view   class="content ql-editor" style="padding:0;" v-html="mainData.content">
					
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
				mainData:{},
				time: 0,
				card:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				var self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					id:self.id
				};
				postData.getAfter = {
					shop:{
						tableName:'Shop',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					card:{
						tableName:'Product',
						middleKey:'passage1',
						key:'product_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
						self.time = self.mainData.limit_time*1000 - new Date().getTime();
						//console.log(new Date().getTime()/1000)
					}
					self.card = JSON.stringify(self.mainData.card[0])
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>
<style>
page{background-color: #63D1F8;}
</style>
<style scoped>
.top{background-color: #1C3741;}
.limitImg{width: 639rpx;height: 437rpx;margin: 50rpx auto 20rpx;}
.gk-icon{width: 23rpx;height: 16rpx;}
.nkImg{width: 150rpx;height: 110rpx;}
.criBtn{width: 160rpx;}
.price{color: #666;}
</style>
