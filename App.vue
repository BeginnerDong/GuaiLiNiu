
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {
			wx.getSystemInfo({
			  success: e => {
			    this.globalData.statusBar = e.statusBarHeight; //状态栏高度
			    let custom = wx.getMenuButtonBoundingClientRect();//菜单按钮
			    this.globalData.custom = custom;
			    this.globalData.customBar = custom.bottom + custom.top - e.statusBarHeight;
			    //计算得到定义的状态栏高度
			  }
			})
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/* 富文本样式 */
	@import "/assets/style/quill.css";
	
	
	/* 公共样式 */
	@import "/common/css/common.css";
	/* 公共样式 */
	@import "/common/css/main.css";
	/* 公共样式 */
	@import "/common/css/animate.css";
	
</style>
