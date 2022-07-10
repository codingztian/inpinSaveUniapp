<template>
	<view class="force-login-wrap">
		<view class="force-login__content y-f">
			<image class="user-avatar" src="../../static/images/about.png" mode="aspectFill"></image>

			<view class="login-boxs">
				<u-form-item :rightIconStyle="{color: '#888', fontSize: '32rpx'}" right-icon="kefu-ermai" :label-position="labelPosition"
				 label="+86 |" prop="phone" >
					<u-input :border="border" placeholder="请输入手机号" v-model="username" type="text"> </u-input>
				</u-form-item>
				<u-form-item :label-position="labelPosition" label="密码" prop="password">
					<u-input :password-icon="true" :border="border" type="password" v-model="password" placeholder="请输入密码"></u-input>
				</u-form-item>
			</view>

			<button class="cu-btn author-btn" @tap="loginUp()">登 录</button>
			<!-- <button class="cu-btn close-btn" @tap="closeAuth">暂不授权</button> -->

			<view class="otherLoginWays">
				<view style="margin: 50px 0">
					<text style="font-size: 14px;font-weight: 400;color: #666666;">
						库管热线<text style="font-size: 11px;font-weight: 400;color: #999999;margin: 0 10px">9:00-20:00</text> | 
					</text>
					<text style="color:#0076E7;font-size:18px;font-weight: bold;margin: 0 10px">400-890-6299</text>
				</view>
				<view class="otherWayTextWrapper">
					<view class="otherWayText">小程序</view>
				</view>
				<view class="icons">
					<slot name="otherLoginWays_icons">
						<!-- <img class="icon" src="https://dian.360cm.net/assets/img/wx.png" @tap="wxlogin()"> -->
					</slot>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '', //用户/电话
				password: '', //密码
				
				
				labelPosition: 'left',
				border: false,
				systeminfo:null,
			}
		},
		onLoad() {
			var _this = this;
			this.getsysteminfo();
			
			
			var userinfo = {};
			userinfo = _this.getUserInfo();
			if (userinfo) {
				if ((userinfo.account != '') && (userinfo.password != '')) {
					_this.loginSever(userinfo.account, userinfo.password)
				}
			}
		},
		methods: {
			getsysteminfo(){
				var _that = this
				//这里请求接口
				// _that._post_form('api/index/getsysteminfo', {}, (result) => {
				// 		_that.systeminfo = result.data;
				// });
			},
			// 点击登录
			loginUp() {
				var _that = this
				if (_that.username == "") {
					uni.showToast({
						title: '请输入手机号',
						icon: 'none',
						duration: 1000
					});
				} else if (_that.password == "") {
					uni.showToast({
						title: '请输入密码',
						icon: 'none',
						duration: 1000
					});
				} else {
					_that.loginSever(_that.username, _that.password)
				}
			},
			loginSever(account, password) {
				var _that = this
				//这里请求接口
				_that._post_form('/api/login', {
					mobile: account,
					password: password,
				}, (result) => {
					// 17701030513
					// kuyidianpassword
					var userinfo = result.data.userinfo;
					userinfo.account = account;
					userinfo.password = password;

					_that.setUserInfo(userinfo);

					uni.setStorageSync('token', userinfo.token)
					uni.setStorageSync('user_id', userinfo.user_id)

					uni.switchTab({
						url: '/pages/statistics/index'
					});

				});
			}

		}
	}
</script>

	<!-- page {
		background: url(../../static/images/loginbg.png) top center / 100% no-repeat;
	} -->

<style lscoped lang="scss">


	.force-login-wrap {
		position: fixed;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		top: 0;

		.logo-bg {
			width: 66px;
			height: 66px;
			position: absolute;
		}

		.lanbg {
			position: relative;
			padding: 15px;
			margin-top: 30px;

			.lan-text {
				margin-left: 5px;
				color: #523e1f;
				height: 22px;
				line-height: 22px;
				position: absolute;
			}
		}

		.y-f {
			display: -webkit-box;
			display: -webkit-flex;
			display: flex;
			flex-direction: column;
			align-items: center;
		}

		.force-login__content {
			position: absolute;
			margin-top: 10vh;
			left: 50%;
			transform: translateX(-50%);

			.user-avatar {
				width: 160rpx;
				height: 160rpx;
				border-radius: 50%;
				overflow: hidden;
				margin-bottom: 40rpx;
			}

			.user-name {
				font-size: 35rpx;
				font-family: PingFang SC;
				font-weight: bold;
				color: rgba(132, 87, 8, 1);
				margin-bottom: 30rpx;
			}

			.login-notice {
				font-size: 28rpx;
				font-family: PingFang SC;
				font-weight: 400;
				color: rgba(200, 150, 61, 1);
				line-height: 44rpx;
				width: 400rpx;
				text-align: center;
				margin-bottom: 80rpx;
			}

			.author-btn {
				width: 630rpx;
				height: 80rpx;
				background: #2982FF;
				box-shadow: 0px 7rpx 6rpx 0px #ccc;
				border-radius: 40rpx;
				font-size: 30rpx;
				font-family: PingFang SC;
				font-weight: 500;
				color: rgba(255, 255, 255, 1);
				margin-top: 20px;
			}

			.close-btn {
				width: 630rpx;
				height: 80rpx;
				margin-top: 30rpx;
				border-radius: 40rpx;
				border: 2rpx solid rgba(233, 180, 97, 1);
				background: none;
				font-size: 30rpx;
				font-family: PingFang SC;
				font-weight: 500;
				color: rgba(233, 180, 97, 1);
			}
		}
	}

	.login-boxs {
		width: 100%;
		height: 100%;
	}

	/*********************第三方登录******************/
	.otherLoginWays {
		margin-top: 95px;
		text-align: center;

		.otherWayTextWrapper {
			line-height: 0;
			color: #666666;

			&:before,
			&:after {
				position: absolute;
				background: #cccccc;
				content: "";
				height: 1px;
				width: 30%;
			}

			&:before {
				left: 0;
			}

			&:after {
				right: 0;
			}

			.otherWayText {
				font-size: .8rem;
				color: #bbbbbb;
			}
		}

		.icons {
			display: flex;
			flex-direction: row;
			justify-content: space-around;
			margin-top: 1.2em;

			img {
				width: 60upx;
				height: 60upx;
			}
		}
	}

	.login_mobile_22 {
		margin-top: 30px;
	}

	.chkAgree {
		white-space: normal;
		width: auto;
		height: 29upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 0upx;
		float: left;
		text-align: left;
		border-radius: 0upx;
		font-size: 28upx;
		line-height: 29upx;
	}

	.login_mobile_22 .login_mobile_24 {
		white-space: normal;
		width: auto;
		height: 29upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 13upx;
		float: left;
		background-color: #ffffff;
		text-align: left;
		border-radius: 0upx;
		color: #646464;
		font-size: 23upx;
		line-height: 29upx;
	}

	.login_mobile_22 .login_mobile_25 {
		white-space: normal;
		width: auto;
		height: 29upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 4upx;
		float: left;
		background-color: #ffffff;
		text-align: left;
		border-radius: 0upx;
		color: #0080ff;
		font-size: 24upx;
		line-height: 29upx;
	}

	.login_mobile_22 .login_mobile_26 {
		white-space: normal;
		width: auto;
		height: 29upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 4upx;
		float: left;
		background-color: #ffffff;
		text-align: left;
		border-radius: 0upx;
		color: #646464;
		font-size: 22upx;
		line-height: 29upx;
	}

	.login_mobile_22 .login_mobile_27 {
		white-space: normal;
		width: auto;
		height: 29upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 6upx;
		float: left;
		background-color: #ffffff;
		text-align: left;
		border-radius: 0upx;
		color: #0080ff;
		font-size: 24upx;
		line-height: 29upx;
	}

	.login_mobile_28 {
		white-space: normal;
		width: 383upx;
		height: 27upx;
		padding: 0upx;
		clear: both;
		margin-top: 5upx;
		float: left;
		text-align: left;
		border-radius: 0upx;
		font-size: 26upx;
		line-height: 27upx;
	}

	.login_mobile_28 .login_mobile_29 {
		white-space: normal;
		width: auto;
		height: 27upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 0upx;
		float: left;
		background-color: #ffffff;
		text-align: left;
		border-radius: 0upx;
		color: #646464;
		font-size: 22upx;
		line-height: 27upx;
	}

	.login_mobile_28 .login_mobile_30 {
		white-space: normal;
		width: auto;
		height: 27upx;
		padding: 0upx;
		margin-top: 0upx;
		margin-left: 11upx;
		float: left;
		background-color: #ffffff;
		text-align: left;
		border-radius: 0upx;
		color: #0080ff;
		font-size: 25upx;
		line-height: 27upx;
	}



	// 注册

	.register {
		font-size: 14px;
		font-family: PingFang SC;
		font-weight: 400;
		line-height: 22px;
		width: 100%;
		margin-top: 10px;
	}
</style>
