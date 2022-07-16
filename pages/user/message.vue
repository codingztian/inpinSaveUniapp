<template>
	<view>
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="我的信息" 
					color="#fff" background-color="rgb(60, 158, 253)"
					@clickLeft="clickLeft" />
			</view>
		</view>
		<view>
			<!-- <image src="../../static/images/none.jpg" class="qiun-none"></image> -->
		</view>
		<view class="userinfo">
			<view class="inview">
				<text>我的程序</text>
				<text>{{userinfo.name}}</text>
			</view>
			<view class="inview">
				<text>用户名称</text>
				<text>{{userinfo.username}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userinfo:"",
			}
		},
		onLoad() {
			let _this = this;
			_this.userinfo = _this.getUserInfo()
		},
		methods: {
			clickLeft() {
				uni.switchTab({url: "/pages/user/index"});
			},
		}
	}
</script>

<style>
	page{background: #FFFFFF;}
	.userinfo {
		font-size: 18px;
    font-weight: 900;
    line-height: 35px;
    padding: 15px;
	}
	.userinfo .inview {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 10px;
		border-bottom: 1px solid #e5e5e5;
	}

.qiun-center{
	display: flex;
	display: -webkit-flex;
	align-items:center;
	justify-content:center;
	padding-top: 100upx;
}
.qiun-none{
	width: 391upx;
	height: 388upx;
}
.qiun-text2{
	font-size: 32upx;
	color: #666666;
}
</style>
