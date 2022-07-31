<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow title="下单成功" 
					color="#fff" background-color="rgb(60, 158, 253)" />
			</view>
		</view>
		<view class="orderOverImg"></view>
		<view style="font-size:16px;color:#666666;text-align: center;">订单提交完成</view>

		<view class="overBtn">
			<view class="btn" @click="switchTab('/pages/order/index')">查看订单</view>
			<view class="btn contu" @click="cutPage('/pages/overbooking/index')">继续发货</view>
		</view>


		
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				shoopInputValue:"",		// 商品数量vmodel
				goodsObj: {},					// 选中的商品对象

			}
		},
		computed:{

		},
		onLoad() {
			_this = this;
			console.log(this.$store.state.orderlist)
			this.$store.state.orderlist = {};
		},
		methods: {
			cutPage(aurl) {
				uni.navigateTo({url: aurl});
			},
			switchTab(aurl) {
				uni.switchTab({
					url: aurl
				});
			},
			
		}
	}
</script>

<style>
	page {background: #F4F5F6;}
	.flexCenter {display: flex;}

	.orderOverImg {
		width: 206px;
		height: 202px;
		margin: 80upx auto 40upx;
		background: url(../../static/images/orderOverImg.png) center / cover no-repeat;
	}
	.overBtn {
		width: 68%;
    margin: 40upx auto;
    display: flex;
    justify-content: space-between;
	}
	.overBtn .btn {
		width: 120px;
		height: 44px;
		line-height: 44px;
		text-align: center;
		background: #fff;
		color: #2982FF;
		font-size: 14px;
		border: 1px solid #2982FF;
	}
	.overBtn .btn.contu {
		background: #2982FF;
		color: #fff;
		border: 1px solid #2982FF;
	}
	
</style>
