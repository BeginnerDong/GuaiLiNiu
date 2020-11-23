<template>
	<view>
		
		<view class="p-r">
			<image src="../../static/images/img2.png" class="img"></image>
			
			<view class="con colorf p-aXY">
				<view>
					<view class="content ql-editor" style="padding:0;" v-html="articleData.content">
						
					</view>
				</view>
				
				<view class="btnTxt p-aX" @click="Router.navigateTo({route:{path:'/pages/experienceInfor/experienceInfor?day='+vipDay}})">立即激活</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				articleData:{},
				vipDay:0
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getArticleData'], self);
		},
		methods: {
			
				
				getArticleData(){
					const self = this;
					const postData = {};
					postData.searchItem = {
						thirdapp_id: 2,
						menu_id: 5,
						title:'新人专享'
					};
					const callback = (res) => {
						if (res.info.data.length > 0) {
							self.articleData = res.info.data[0];
							self.vipDay = self.articleData.view_count;
						}
						self.$Utils.finishFunc('getArticleData');
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
.img{width: 630rpx;height: 956rpx;margin: 15% auto;}
.con{margin: 60% 112rpx 0;}
.btnTxt{text-align: center;bottom: 30%;}
</style>
