<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" shadow title="订单管理" 
					color="#fff" background-color="rgb(60, 158, 253)" />
			</view>
		</view>
		<view style="height:6vh;background: #fff;display: flex;align-items: center;justify-content: center;">
			<view class="crOrder">
				<view class="crOrderBtn" :class="{'curr':crOrderIndex==1}" @click="cutCrOrder(1)">入库订单</view>
				<view class="crOrderBtn" :class="{'curr':crOrderIndex==2}" @click="cutCrOrder(2)">出库订单</view>
			</view>
		</view>
		<view>
			<view class="example-body">
				<uni-datetime-picker v-model="range" type="daterange" @maskClick="maskClick" />
			</view>
		</view>

		<view style="background: #fff;">
			<view class="orderlist">
				<view v-for="(cell,k) in orederList" :key="k" class="goods__item">
				<view class="flexCenter">
					<view style="margin-right: 15px;" class="flexCenter radius5px"><img src="/static/images/200.png" alt="51" style="width:100px;height:100px;"></view>
				</view>
				<view style="border-bottom: 1px solid #EEEEEE;height: 110px;flex: 1;display: flex;flex-direction: column;justify-content: space-between;">
					<view>
						<view style="font-size:16px;color:#333333;font-weight:900;margin-bottom: 7px;">{{ cell.label }}</view>
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
				crOrderIndex: 1,
				range: [],
				orederList: [
					{label: "猪肉",id: "1",kw: "36号",num: "3600",},{label: "牛肉",id: "1",kw: "36号",num: "3600",},{label: "肌肉",id: "1",kw: "36号",num: "3600",},
					{label: "猪肉",id: "1",kw: "36号",num: "3600",},{label: "牛肉",id: "1",kw: "36号",num: "3600",},{label: "肌肉",id: "1",kw: "36号",num: "3600",},
					{label: "猪肉",id: "1",kw: "36号",num: "3600",},{label: "牛肉",id: "1",kw: "36号",num: "3600",},{label: "肌肉",id: "1",kw: "36号",num: "3600",},
					{label: "猪肉",id: "1",kw: "36号",num: "3600",},{label: "牛肉",id: "1",kw: "36号",num: "3600",},{label: "肌肉",id: "1",kw: "36号",num: "3600",},
				],
			}
		},
		onLoad() {
			_this = this;
			// 获取今天时间 yyyy-MM-dd
			var date = new Date();
			var year = date.getFullYear();
			var month = date.getMonth() + 1;
			var day = date.getDate();
			var today = year + '-' + month + '-' + day;
			this.range = [today,today]
		},
		watch: {
			range(newval) {
				console.log('范围选:', this.range);
			}
		},
		methods: {
			cutCrOrder(val) {
				this.crOrderIndex = val;
			},
			maskClick(e){
				console.log('maskClick事件:', e);


			}
		}
	}
</script>

<style lang="less">
	/deep/ .uni-navbar__content  {height: 6vh;}
	/deep/ .uni-nav-bar-text {font-size: 16px;}
	/deep/ .example-body .uni-date__x-input {height: 5vh;}
	/deep/ .uni-date-x--border {border: none;border-radius: 0;box-sizing: border-box;}
</style>
<!-- border-bottom: 1px solid #dcdfe6;border-top: 1px solid #dcdfe6;
 -->
<style>
	page {background: #F4F5F6;}

	.crOrder {
		width: 95%;
		height: 70%;
		color: #2982FF;
		font-size: 14px;
		background: #fff;
		border: 1px solid #2982FF;
		border-radius: 5px;
		display: flex;
	}
	.crOrder .crOrderBtn {
		flex: 1;
		display: flex;
		align-items: center;
    	justify-content: center;
	}
	.crOrder .crOrderBtn.curr {
		background: #2982FF;
		color: #fff;
	}
	.example-body {}

	.orderlist {
		height: 77vh;
		padding:8px 12px;
		overflow: auto;
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