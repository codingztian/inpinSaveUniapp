<script>
	export default {
		// tabble 中间按钮
		// 代码位置：App.vue
		onLaunch: function() { //APP 初始化完成
			uni.onTabBarMidButtonTap(()=>{
				console.log("点击了")
		        // 这里可以根据 个人需求 做点击处理，
		        // 本人需进行页面跳转。
				uni.navigateTo({
					url:"/pages/overbooking/index",
					animationType:'slide-in-bottom'
				})
			})
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="less">
	/deep/ .uni-navbar__content  {height: 6vh;}
	/deep/ .uni-nav-bar-text {font-size: 16px;}
</style>

<style lang="scss">
	@import "uview-ui/index.scss";
	@import "utils/_utils.scss";
	/*每个页面公共css */
	page {
		background-color: #fff;
	}
	
	.white{
		background-color: #fff;
	}
	
	.wrap {
		padding: 30rpx;
		background-color: #fff;
	}
	
	.font-weight-bold{
		font-weight: bold;	
	}
	
	.uni-tabbar__mid {
		background:linear-gradient(-125deg, #006AFF, #2982FF);;
		border-radius: 50px;
	}
	.uni-tabbar__mid +.uni-tabbar__bd .uni-tabbar__icon {
		display:none;
	}
	.uni-tabbar__mid +.uni-tabbar__bd .uni-tabbar__label {
		padding: 8px;
		color: #fff !important;
		font-size: 14px !important;
		line-height: 1.3 !important;
	}
	
	.uni-tabbar-bottom .uni-tabbar {height: 6vh;}

	.uni-app--showtabbar uni-page-wrapper::after {
		height: 6vh !important;
	}
	

</style>

