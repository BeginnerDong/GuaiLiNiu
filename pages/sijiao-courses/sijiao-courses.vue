<template>
	<view>
		
		<view class="shadow radius20 m-a p-r mb-3 tkBox" @click="Router.navigateTo({route:{path:'/pages/sijiao-classDetail/sijiao-classDetail'}})">
			<image src="../../static/images/sijiao-img.png" class="kcImg"></image>
			<view class="px-2 py-3 font-30 line-h">
				<view class="font-w pb-3">减脂训练营·中上</view>
				<view><text class="price font-w">220</text>/9课时</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
