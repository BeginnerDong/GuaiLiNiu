<template>
	<view>
		
		<view class="px-2 pt-3">
			<view class="flex1 bB-f5 py-4">
				<image src="../../static/images/about us-icon.png" class="wh50"></image>
				<view class="font-28 color2 flex-1 pl-2">客服微信</view>
				<view>{{mainData.keywords?mainData.keywords:''}}</view>
			</view>
			<view class="flex1 bB-f5 py-4">
				<image src="../../static/images/about us-icon1.png" class="wh50"></image>
				<view class="font-28 color2 flex-1 pl-2">联系电话</view>
				<view>{{mainData.phone?mainData.phone:''}}</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<view class="px-2 py-3">
			<view>
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
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				var self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					menu_id:3
				};
				var callback = function(res){
					if(res.info.data.length>0){
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>

<style scoped>
.img{height: 400rpx;}
</style>
