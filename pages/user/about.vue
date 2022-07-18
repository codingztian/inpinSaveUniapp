<template>
	<view class="about">
		<view class="content">
			<view class="box-bg">
				<view class="box-bg uni-nav-bar">
					<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="关于我们" 
						color="#fff" background-color="rgb(60, 158, 253)"
						leftText="返回" @clickLeft="clickLeft" />
				</view>
			</view>
			<view class="qrcode">
				<image class="qrcodeImg" src="../../static/images/about.png" @longtap="save"></image>
				<text class="tip">库易点</text>
				<view class="version">{{version}}
		</view>
			</view>
			<view class="desc">
				<view class="descSpan" style="border-bottom:1px solid #F4F5F6;">
					<text class="s1">联系我们</text>
					<text class="s2">13353421184</text>
				</view>
				<view class="descSpan">
					<text class="s1">隐私协议</text>
					<view class="s2" @click="cutPage('/pages/user/service')">
						<uni-icons type="right" size="30"></uni-icons>
					</view>
				</view>
			</view>
			
			<!-- 			
			<view class="source">
				<view class="title">本示例源码获取方式：</view>
				<view class="source-list">
					<view class="source-cell">
						<text space="nbsp">1. </text>
						<text>下载 HBuilderX，新建 uni-app 项目时选择 <text class="code">uCharts</text> 模板。</text>
					</view>
					<view class="source-cell">
						<text space="nbsp">2. </text><text @click="openLink" class="link">{{sourceLink}}</text>
					</view>
				</view>
			</view> -->

		</view>
		<!-- #ifdef APP-PLUS -->
		
		<!-- #endif -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				providerList: [],
				version: 'V1.0.1',
				sourceLink: 'https://www.ucharts.cn'
			}
		},
		onLoad() {
			// #ifdef APP-PLUS
			this.version = plus.runtime.version;
			uni.getProvider({
				service: 'share',
				success: (result) => {
					const data = [];
					for (let i = 0; i < result.provider.length; i++) {
						switch (result.provider[i]) {
							case 'weixin':
								data.push({
									name: '分享到微信好友',
									id: 'weixin'
								});
								data.push({
									name: '分享到微信朋友圈',
									id: 'weixin',
									type: 'WXSenceTimeline'
								});
								break;
							case 'qq':
								data.push({
									name: '分享到QQ',
									id: 'qq'
								});
								break;
							default:
								break;
						}
					}
					this.providerList = data;
				},
				fail: (error) => {
					console.log('获取登录通道失败' + JSON.stringify(error));
				}
			});
		// #endif
		},
		methods: {
			clickLeft() {
				uni.switchTab({url: '/pages/user/index'});
			},
			cutPage(url) {
				uni.navigateTo({url: url});
			},
			// #ifdef APP-PLUS
			save() {
				uni.showActionSheet({
					itemList: ['保存图片到相册'],
					success: () => {
						plus.gallery.save('../../static/images/qrcode.png', function() {
							uni.showToast({
								title: '保存成功',
								icon: 'none'
							});
						}, function() {
							uni.showToast({
								title: '保存失败，请重试！',
								icon: 'none'
							});
						});
					}
				});
			},
			
			// #endif
			openLink() {
				// #ifdef APP-PLUS
				plus.runtime.openURL(this.sourceLink);
				// #endif
				// #ifdef H5
				window.open(this.sourceLink);
				// #endif
			}
		}
	}
</script>

<style>
	page {
		min-height: 100%;
		background-color: #f4f5f6;
	}

	image {
		width: 360upx;
		height: 360upx;
	}

	.about {
		flex-direction: column;
		flex: 1;
	}

	.content {
		flex-direction: column;
		justify-content: center;
	}

	.qrcode {
		margin: 5vh 0;
		display: flex;
		align-items: center;
		flex-direction: column;
	}
	.qrcode .qrcodeImg {
		width: 66px;
		height: 66px;
	}

	.qrcode .tip {
		font-size: 16px;
		color: #333333;
		margin-top: 20upx;
	}

	.desc {
		display: block;
		background: #fff;
		line-height: 100upx;
	}
	.desc .descSpan {display: flex;justify-content: space-between;}
	.desc .descSpan .s1 {width: 60px;color: #666666;margin: 0 24upx; }
	.desc .descSpan .s2 {color: #666666;margin: 0 24upx; }

	.code {
		color: #e96900;
		background-color: #f4f5f6;
	}

	button {
		width: 100%;
		margin-top: 40upx;
	}

	.version {
		color: #999999;
		font-size: 12px;
		height: 80upx;
		line-height: 80upx;
		justify-content: center;
		color: #ccc;
	}

	.source {
		margin-top: 30upx;
		flex-direction: column;
	}

	.source-list {
		flex-direction: column;
	}

	.link {
		color: #007AFF;
	}
</style>
