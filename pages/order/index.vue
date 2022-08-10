<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar title="订单管理" statusBar=true height="6vh"
					color="#fff" background-color="rgb(60, 158, 253)" />
			</view>
		</view>
		<view style="height:8vh;background: #fff;display: flex;align-items: center;justify-content: center;">
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
			<scroll-view  class="orderlist scroll" scroll-y="true" @scrolltolower="lower()" >
				<view v-if="orederList.length">
					<view>
						<view v-for="(cell,key) in orederList" :key="key">
							<view style="background: #f8f8f8;padding: 12px 10px 7px;margin-bottom: 10px;color: #333;">
								<text>{{cell.time}}</text>
								<text style="float:right;">{{ crOrderIndex==2?'收货方：'+cell.kehu:''}}</text>
							</view>
							<view v-for="(item,index) in cell.goods" :key="index" class="goods__item">
								<view style="width:100%;margin: 6px 0px;display: flex;justify-content: space-between;font-size: 12px;">
									<view style="font-size: 13px;">订单号：{{cell.order}}</view>
									<view :class="item.status.state==1?'bc3711':'f3a73f'" v-if="crOrderIndex==2">
										<text>{{item.status.txt}}</text>
										<text style="color:#999;margin-left: 5px;">{{item.add_time}}</text>
									</view>
									<view style="color:#999;margin-left: 5px;" v-else>入库时间：{{item.add_time}}</view>
								</view>
								<view class="flexCenter">
									<view style="margin-right: 15px;" class="flexCenter radius5px"><img :src="item.thumb_img" alt="51" style="width:80px;height:80px;"></view>
								</view>
								<view style="border-bottom: 1px solid #EEEEEE;min-height: 110px;flex: 1;display: flex;flex-direction: column;justify-content: space-between;">
									<view>
										<view style="font-size:16px;color:#333333;font-weight:900;margin-bottom: 7px;">{{ item.name }}</view>
										<view style="color:#666666;display: flex;font-size: 12px;min-height: 20px;" v-if="crOrderIndex==1">
											<text style="display: inline-block;color:#999999;width:50px;">供应商</text>
											<text style="flex: 1;">{{item.factory_name}}</text>
										</view>
										<view style="color:#666666;display: flex;font-size: 12px;min-height: 20px;" v-if="crOrderIndex==1">
											<text style="display: inline-block;color:#999999;width:50px;">库位</text> 
											<text style="flex: 1;">{{item.location}}</text>
										</view>
										<view style="color:#666666;display: flex;font-size: 12px;min-height: 20px;" v-if="crOrderIndex==2">
											<text style="display: inline-block;color:#999999;width:50px;">供应商</text>
											<text style="flex: 1;">{{item.factory_name}}</text>
										</view>
									</view>
									<view style="display:flex;align-items: flex-end;justify-content: space-between;margin-bottom: 10px;">
										<view style="color:#FF4C4B;font-size:14px;">
											<text style="font-weight:900;">{{item.num}}</text>
											<text style="display: inline-block;font-size:12px;color:#999999;margin-left:5px;">{{item.unit_name}}</text>
										</view>
										<view style="color:#ff4c4b;font-size:13px;margin-top: 5px;">¥{{parseInt(item.price).toFixed(2)}}</view>
									</view>
								</view>
							</view>
						</view>
					</view>
					<view><uni-load-more iconType="auto" :status="status" v-if="lodingStatus"/></view>
				</view>
				<view v-else style="padding-top: 18vh;">
					<u-empty mode="order" icon="http://cdn.uviewui.com/uview/empty/order.png"  ></u-empty>
				</view>
			</scroll-view>
		</view>

	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				crOrderIndex: 2,
				range: [],
				orederList: [],
				page:1,
				pageinfo:{},
				status: 'loading',
				lodingStatus: false,
			}
		},
		onShow(e) {
			_this = this;
			_this.crOrderIndex = 2;
			// console.log(e.crOrderIndex);
			// if(e.crOrderIndex) _this.crOrderIndex = e.crOrderIndex;
			var startTime = new Date().getTime();
			var sevenDay = 1000 * 60 * 60 * 24 * 1;
			_this.range = [_this.timeTrans(new Date(startTime - sevenDay)),_this.timeTrans(new Date())];
		},
		watch: {
			range(newval) {
				// console.log('范围选:', this.range);
				this.orederList = [];
				this.page = 1;
				_this.getOrderInfo();
			}
		},
		methods: {
			cutCrOrder(val) {
				if(val!=this.crOrderIndex) {
					this.crOrderIndex = val;
					this.orederList = [];
					this.page = 1;
					_this.getOrderInfo();
				}
			},
			getOrderInfo() {
				let _this = this;
				if(_this.page==1) uni.showLoading({mask:true,title: '订单载入中'});

				_this._post_form('/api/order/orderlist', {
					start_date: _this.range[0],
					end_date: _this.range[1],
					type: _this.crOrderIndex,
					page: _this.page
				},
				res=> {
					if(res.errno==0) {
						_this.orederList = _this.orederList.concat(res.data.list);
						_this.pageinfo = res.data.pageinfo;
						console.log(_this.orederList);
					}
					let s = setTimeout(()=>{
						clearTimeout(s);
						uni.hideLoading();
					},800);
				});
			},
			lower(e) {
				if(this.pageinfo.page==this.pageinfo.pageCount) return false;
				if(this.lodingStatus) return false;
				this.lodingStatus = true;
				let set = setTimeout(() => {
					clearTimeout(set);
					this.page++;
					this.lodingStatus = false;
					this.getOrderInfo()
				}, 3000)
			},
			timeTrans(date) {
				var year = date.getFullYear();
				var month = date.getMonth() + 1;
				var day = date.getDate();
				return year + '-' + month + '-' + day;
			},
			maskClick(e){
				console.log('maskClick事件:', e);
			}
		}
	}
</script>

<style lang="less">
	// /deep/ .uni-navbar__content  {height: 6vh;}
	// /deep/ .uni-nav-bar-text {font-size: 16px;}
	/deep/ .example-body .uni-date__x-input {height: 5vh;
		// border-bottom: 1px solid #eee;
	}
	/deep/ .uni-date-x--border {border: none !important;border-radius: 0;box-sizing: border-box;}
</style>
<!-- border-bottom: 1px solid #dcdfe6;border-top: 1px solid #dcdfe6;
 -->
<style>
	.bc3711{color: #18bc37;}
	.f3a73f{color: #f3a73f;}
	.e43d33{color: #e43d33;}
	.f939c1{color: #8f939c;}
	/* page {background: #F4F5F6;} */

	.crOrder {
		width: 95%;
		height: 70%;
		color: #2982FF;
		font-size: 14px;
		background: #fff;
		/* border: 1px solid #2982FF; */
		border-radius: 5px;
		display: flex;
		/* overflow: hidden; */
	}
	.crOrder .crOrderBtn {
		flex: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 1px solid #2982FF;
		border-radius: 5px;
		font-size: 18px;
	}
	.crOrder .crOrderBtn:nth-child(1) {
		border-top-right-radius: 0px;
		border-bottom-right-radius: 0px;
	}
	.crOrder .crOrderBtn:nth-child(2) {
		border-top-left-radius: 0px;
		border-bottom-left-radius: 0px;
	}

	.crOrder .crOrderBtn.curr {
		background: #2982FF;
		color: #fff;
	}
	.example-body {}

	.orderlist {
		height: 77vh;
		/* padding:8px 12px; */
		overflow: auto;
		box-sizing: border-box;
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
		margin: 0 22upx 24upx;
		display: flex;
		align-items: flex-start;
		justify-content: flex-start;
		flex-wrap: wrap;
	}
	
</style>