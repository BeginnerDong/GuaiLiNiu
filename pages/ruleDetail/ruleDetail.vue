<template>
	<view class="px-2 py-3" v-html="serviceData.content">
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				serviceData:{},
				searchItem:{
					menu_id: 5,
					thirdapp_id: 2
				}
			}
		},
		onLoad(options) {
			const self = this;
			if(options.type==0){
				self.searchItem.title = '怪力牛运动团课购买协议书'
			}else if(options.type==1){
				self.searchItem.title = '怪力牛运动私教课程购买协议书'
			}
			self.getServiceData();
		},
		methods: {
			
			getServiceData(){
				const self = this;
				const postData = {};
				postData.searchItem = self.searchItem;
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.serviceData =  res.info.data[0];
					};
					uni.setStorageSync('canClick', true);
					self.$Utils.finishFunc('getServiceData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
		}
	}
</script>

<style>

</style>
