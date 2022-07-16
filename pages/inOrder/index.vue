<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="入库管理" 
					color="#fff" background-color="rgb(60, 158, 253)"
					@clickLeft="clickLeft" />
			</view>
		</view>
		<view>
			<view class="example-body">
				<uni-datetime-picker style="border:none;" v-model="range" type="daterange" @maskClick="maskClick" />
			</view>
		</view>

		<view class="orderlist">
			<view>
				<checkbox-group class="block" @change="changeCheckbox">
					<view v-for="(item,k) in orederList" :key="k" class="goods__item">
						<view style="height: 100px;line-height: 100px;">
							<checkbox :value="String(item.value)"
								:checked="checkedArr.includes(String(item.value))"
								:class="{'checked':checkedArr.includes(String(item.value))}"></checkbox>
						</view>
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
				</checkbox-group>

			</view>
		</view>

		<view style="width: 100%;height: 7vh;display: flex;justify-content: space-between;align-items: center;padding: 10px;padding-left: 12px;background-color: rgb(255, 255, 255);box-shadow: rgb(204 204 204) 0px -1px 10px 0px;font-size: 16px;color: #666666;position: fixed;bottom: 0;z-index: 1;">
			<view style="display: flex;align-items: center;">
				<checkbox-group @change="allChoose" class="checkboxStyle">
					<checkbox value="all" :class="{'checked':allChecked}" :checked="allChecked?true:false"></checkbox>
				</checkbox-group>
				<view>共<text style="margin: 0 3px;color: #FF4C4B;">{{checkedArr.length}}</text>件</view>
			</view>
			<text @click="toOredrOver" style="height: 100%;width:100px;display: flex;align-items: center;justify-content: center;border-radius: 5px;padding: 2px 10px;font-weight: normal;cursor: pointer;background: #2982FF;border-radius: 100px;color: #fff;">
				提交
			</text>
		</view>

	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				range: [],
				orederList: [
					{label: "猪肉",value: "1",kw: "36号",num: "3600",},{label: "牛肉",value: "2",kw: "36号",num: "3600",},{label: "肌肉",value: "3",kw: "36号",num: "3600",},
					{label: "猪肉",value: "4",kw: "36号",num: "3600",},{label: "牛肉",value: "5",kw: "36号",num: "3600",},{label: "肌肉",value: "6",kw: "36号",num: "3600",},
					{label: "猪肉",value: "7",kw: "36号",num: "3600",},{label: "牛肉",value: "8",kw: "36号",num: "3600",},{label: "肌肉",value: "9",kw: "36号",num: "3600",},
					{label: "猪肉",value: "10",kw: "36号",num: "3600",},{label: "牛肉",value: "11",kw: "36号",num: "3600",},{label: "肌肉",value: "12",kw: "36号",num: "3600",},
				],

				isChecked:false,
				
				checkedArr:[], //复选框选中的值
				allChecked:false //是否全选
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
			clickLeft() {
				uni.switchTab({url: "/pages/statistics/index"});
			},
			// 多选复选框改变事件
			changeCheckbox(e) {
					this.checkedArr = e.detail.value;
					// 如果选择的数组中有值，并且长度等于列表的长度，就是全选
					if (this.checkedArr.length > 0 && this.checkedArr.length == this.orederList.length) {
							this.allChecked = true;
					} else {
							this.allChecked = false;
					}
					console.log(this.checkedArr);
			},
			// 全选事件
			allChoose(e) {
					let chooseItem = e.detail.value;
					// 全选
					if (chooseItem[0] == 'all') {
							this.allChecked = true;
							for (let item of this.orederList) {
									let itemVal = String(item.value);
									if (!this.checkedArr.includes(itemVal)) {
											this.checkedArr.push(itemVal);
									}
							}
						console.log(this.checkedArr);
					} else {
							// 取消全选
							this.allChecked = false;
							this.checkedArr = [];
					}
			},
			// 提交
			toOredrOver() {
				uni.showLoading({title: '提交账单中'});
				setTimeout(() => {
					uni.hideLoading()
					uni.navigateTo({
						url: '/pages/shoppingCart/over'
					})
				}, 300)
			},
			maskClick(e){
				console.log('maskClick事件:', e);
			},


		}
	}
</script>


<style>
	page {background: #F4F5F6;}

	.orderlist {
		height: 82vh;
		padding:8px 12px;
		background: #fff;
		border-top: 1px solid #eee;
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

<style lang="less">
	/deep/ .uni-date-x--border {border: none;}

	/deep/ uni-checkbox .uni-checkbox-input {
		border-radius: 22px;margin-right: 12px;
	}
	/deep/ uni-checkbox .uni-checkbox-input.uni-checkbox-input-checked {
		border-color: #007aff !important;
	}
	/deep/ uni-checkbox:not([disabled]) .uni-checkbox-input:hover {
		border-color: #d1d1d1;
	}
</style>