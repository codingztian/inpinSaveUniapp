<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" shadow left-icon="left" title="库存预警" 
					color="#fff" background-color="rgb(60, 158, 253)"
					@clickLeft="clickLeft" />
			</view>
		</view>
		
		<view class="orderlist">
			<view class="orderlistBox">
				<view v-for="(item,k) in orederList" :key="k" class="goods__item">
					<view class="flexCenter">
						<view style="margin-right: 15px;" class="flexCenter radius5px"><img src="/static/images/200.png" alt="51" style="width:100px;height:100px;"></view>
					</view>
					<view style="border-bottom: 1px solid #EEEEEE;height: 110px;flex: 1;display: flex;flex-direction: column;justify-content: space-between;">
						<view>
							<view style="font-size:16px;color:#333333;font-weight:900;margin-bottom: 7px;">{{ item.label }}</view>
							<view style="color:#666666;"><text style="display: inline-block;color:#999999;width:50px;line-height: 20px;">供应商</text>湖北武汉<text></text></view>
							<view style="color:#666666;"><text style="display: inline-block;color:#999999;width:50px;line-height: 20px;">库位</text>A区36号</view>
						</view>
						<view style="display:flex;align-items: flex-end;justify-content: space-between;margin-bottom: 10px;">
							<view style="color:#FF4C4B;font-size:14px;">3600<text style="display: inline-block;font-size:12px;color:#999999;margin-left:5px;">件</text></view>
						</view>
					</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				orederList: [
					{label: "猪肉",value: "1",kw: "36号",num: "3600",},{label: "牛肉",value: "2",kw: "36号",num: "3600",},{label: "肌肉",value: "3",kw: "36号",num: "3600",},
					{label: "猪肉",value: "4",kw: "36号",num: "3600",},{label: "牛肉",value: "5",kw: "36号",num: "3600",},{label: "肌肉",value: "6",kw: "36号",num: "3600",},
					{label: "猪肉",value: "7",kw: "36号",num: "3600",},{label: "牛肉",value: "8",kw: "36号",num: "3600",},{label: "肌肉",value: "9",kw: "36号",num: "3600",},
					{label: "猪肉",value: "10",kw: "36号",num: "3600",},{label: "牛肉",value: "11",kw: "36号",num: "3600",},{label: "肌肉",value: "12",kw: "36号",num: "3600",},
				],
			}
		},
		onLoad() {
			_this = this;
			
		},
		watch: {
			
		},
		methods: {
			clickLeft() {
				uni.switchTab({url: "/pages/statistics/index"});
			},
		}
	}
</script>

<style>
	page {background: #fff;}

	.orderlist {
		height: 94vh;
		border: 10px solid #fff;
		box-sizing: border-box;
		overflow: auto;
	}
	.orderlistBox {
		padding-bottom: 0px;
	}

	.flexCenter {display: flex;}
	.radius5px {border-radius: 5px;overflow: hidden;}
	.goods__item {
		padding: 0upx;
		box-sizing: border-box;
		background-color: #ffffff;
		border-radius: 4px;
		font-size: 14px;
		color: #262626;
		margin-bottom: 24upx;
		display: flex;
		align-items: flex-start;
		justify-content: flex-start;
	}





</style>

<style lang="less">
	/deep/ .uni-navbar__content  {height: 6vh;}
	/deep/ .uni-nav-bar-text {font-size: 16px;}
</style>