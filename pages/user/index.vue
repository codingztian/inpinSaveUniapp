<template>
	<view class="center">
		<!-- <view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" shadow title="" 
					color="#fff" background-color="rgb(33,130,255)" />
			</view>
		</view> -->

		<view class="logo" @click="goLogin" :hover-class="!login ? 'logo-hover' : ''">
			<image class="logo-img" src="../../static/images/user/myPic.png"></image>
			<view class="logo-title">
				<text class="uer-name">{{login ? userInfo.username : '您未登录'}}</text>
				<text class="uni-panel-icon uni-icon" v-if="!login">&#xe581;</text>
				<view @click="loginout">注销</view>
			</view>
		</view>
		

		<view class="info" style="margin-bottom:26px;margin-top: -30px;">
			<view style="margin-bottom:12px;">信息管理</view>
			<view class="flex" style="justify-content: space-between;">
				<view class="io myinfo" @click="cutPage('/pages/user/about')">
					<view class="pic"></view>
					个人信息
				</view>
				<view class="io userinfo" @click="cutPage('/pages/user/userinfo')">
					<view class="pic"></view>
					客户管理
				</view>
				<view class="io customerInfo" @click="cutPage('/pages/user/supplierlist')">
					<view class="pic"></view>
					供应商
				</view>
				<view class="io mykg" @click="cutPage('/pages/user/index')">
					<view class="pic"></view>
					我的库管
				</view>
			</view>
		</view>
		<view class="info">
			<view style="margin-bottom:12px;">信息管理</view>
			<view class="flex">
				<view class="io about" style="margin-right: 35px;" @click="cutPage('/pages/user/about')">
					<view class="pic"></view>
					关于我们
				</view>
				<view class="io set" @click="cutPage('/pages/user/about')">
					<view class="pic"></view>
					设置
				</view>
			</view>
		</view>

		<!-- 		
		<u-cell-group class="m_top20">
			<u-cell-item icon="account-fill" color='#22c1a9' title="帐号管理" @click="gourl('account')"></u-cell-item>
			<u-cell-item icon="chat-fill" color='#f3e5e9' title="新消息通知" @click="gourl('message')"></u-cell-item>
			<u-cell-item icon="question-circle-fill" color='#aaaaff' title="问题反馈" @click="gourl('feedback')"></u-cell-item>
			<u-cell-item icon="zhuanfa" color='#aa557f' title="服务条款及协议" @click="gourl('service')"></u-cell-item>
			<u-cell-item icon="error-circle-fill" color='#dc6a19' title="关于应用" @click="gourl('about')"></u-cell-item>
			
		</u-cell-group>
		
		<u-cell-group class="m_top20">
			<button open-type="contact" class="white btn">
				<u-cell-item icon="weixin-fill" color='#189f33' title="在线客服"></u-cell-item>
			</button>
			<u-cell-item icon="qq-fill" color='#189f33' title="在线客服" @click="onChat()"></u-cell-item>
		</u-cell-group>
		 -->
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				login: false,
				userInfo: {}
			}
		},
		onLoad() {
			_this = this;
			let userinfo = uni.getStorageSync('userinfo');
			if (userinfo) {
				this.login=true;
				this.userInfo = userinfo;
			}
			this.getUserDetail();

			console.log(uni.getStorageSync('userinfo'));
		},
		methods: {
			loginout() {
				_this._post_form('/api/login/loginout', {}, (result) => {
					console.log(result);
					// _this.data = result.data
				});
			},
			cutPage(url) {
				uni.navigateTo({
					url: url
				})
			},

			getUserDetail() {
				var _this = this;
				// _this._post_form('api/user/getUserDetail', {}, function(result) {
				// 	_this.setData({
				// 		userInfo:result.data
				// 	})
				// });
			},
			goLogin() {
				if (!this.login) {
					uni.navigateTo({
						url: 'login'
					});
				}
			},
			gourl(url){
				if (this.login) {
					uni.navigateTo({
						url: url
					});
				}else{
					uni.navigateTo({
						url: 'login'
					});
				}
			},
			onChat(){
				plus.runtime.openURL('mqqwpa://im/chat?chat_type=crm&uin=2903475819',
					function(res) {
						plus.nativeUI.alert("本机没有安装QQ，无法启动");
					});
			}
		}
	}
</script>

<style>
	page {background-color: #F4F5F6;}
	.m_top20{margin-top: 20px;}
	.center {flex-direction: column;}
	.flex{display: flex;text-align: center;}

	.logo {
		height: 235px;
		padding: 70px 27px 0px;
		background-color: #2fc25b;
		background: url(../../static/images/user/userbg.png) center / cover no-repeat;
		display: flex;
	}

	.logo-hover {
		opacity: 0.8;
	}

	.logo-img {
		width: 150upx;
		height: 150upx;
		border-radius: 150upx;
	}

	.logo-title {
		height: 150upx;
		flex: 1;
		align-items: center;
		justify-content: space-between;
		flex-direction: row;
		margin-left: 20upx;
	}

	.info {
		height: 137px;
		border-radius: 10px;
		background: #fff;
		margin: 12px;
		padding: 14px 25px 13px;
		box-sizing: border-box;
		font-size: 14px;
		color: #333333;
		position: relative;
    z-index: 1;
	}
	.info .io .pic {
		width: 60px;
		height: 60px;
		color: #666666;
	}
	.info .io.myinfo .pic {background: url(../../static/images/user/myInfo.png) center no-repeat;}
	.info .io.userinfo .pic {background: url(../../static/images/user/userInfo.png) center no-repeat;;}
	.info .io.customerInfo .pic {background: url(../../static/images/user/customerInfo.png) center no-repeat;}
	.info .io.mykg .pic {background: url(../../static/images/user/myKg.png) center no-repeat;}
	.info .io.about .pic {background: url(../../static/images/user/about.png) center no-repeat;}
	.info .io.set .pic {background: url(../../static/images/user/set.png) center no-repeat;}


	.uer-name {
		height: 60upx;
		line-height: 60upx;
		font-size: 38upx;
		color: #FFFFFF;
		line-height: 75px;
	}
	
	.btn{
		position: initial;
		padding-left: 0px;
		padding-right: 0px;
	}


</style>
