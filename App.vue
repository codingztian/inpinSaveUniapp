<script>
	export default {
		onLaunch: function(e) { //APP 初始化完成
			// tabble 中间按钮
			// 代码位置：App.vue
			// uni.onTabBarMidButtonTap(()=>{
			// 	console.log("点击了")
		  //       // 这里可以根据 个人需求 做点击处理，
		  //       // 本人需进行页面跳转。
			// 	uni.navigateTo({
			// 		url:"/pages/overbooking/index",
			// 		animationType:'slide-in-bottom'
			// 	})
			// })

			// console.log('App Launch：初始化完成（全局只触发一次）');
			// console.log("userInfo：",uni.getStorageSync('userinfo'));
		},
		onShow: function(e) {
			// console.log(e);
			// console.log('App Show：','页面启动完成')
			if (e.path !== 'pages/login/index' && !uni.getStorageSync('token')) {
				uni.navigateTo({url:"/pages/login/index",})
			}
		},
		onHide: function() {
			// console.log('App Hide：已进入后台运行')
		}
		
	}
</script>

<style lang="less">
	// .box-bg {height: 9vh;background: rgb(60, 158, 253);}
	// /deep/ .uni-navbar__content  {height: 8vh;}
	.uni-status-bar {height: 4vh !important;}
	/deep/ .uni-navbar__content  {border: none !important;}
	/deep/ .uni-nav-bar-text {font-size: 18px !important;}
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

	.cp-goods-select-category-item.actived {
		color: #2982FF !important;
		background-color: #fff !important;
	}
	.cp-goods-select-category-item.actived::before {
		background-color: transparent !important;
	}
	.cp-goods-select-goods__classify {
		line-height: 42px !important;
		background-color: #fff !important;
	}

</style>

