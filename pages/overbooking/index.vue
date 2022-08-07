<template>
	<view class="content" v-if="list">
			<view class="box-bg">
				<view class="box-bg uni-nav-bar">
					<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="选择分类" 
						color="#fff" background-color="rgb(60, 158, 253)"
						@clickLeft="clickLeft" />
				</view>
			</view>

			<!-- 商品分类 -->
			<view class="goods-select">
				<cp-goods-select
					height="83vh"
					:props="{label:'name',value:'id',children:'children'}" 
					:options="list"
					@scrolltolower="handelScrolltolower" 
					@category-change="handelCategoryChange">
					<cp-goods-item v-for="(item,index) in list" :key="index" :category="item.name">
						<view v-for="(cell,k) in item.children" :key="k" class="goods__item">
							<view class="flexCenter">
								<view style="margin-right: 15px;" class="flexCenter radius5px"><img :src="cell.thumb_img" alt="51" style="width:80px;height:80px;"></view>
							</view>
							<view style="border-bottom: 1px solid #EEEEEE;min-height: 110px;flex: 1;display: flex;flex-direction: column;justify-content: space-between;">
								<view>
									<view style="font-size:16px;color:#333333;font-weight:900;margin-bottom: 7px;">{{ cell.name }}</view>
									<view style="color:#666666;display: flex;align-items: center;font-size: 12px;">
										<text style="display: inline-block;color:#999999;width:40px;line-height: 20px;">供应商</text>
										<text>{{cell.factory_name}}</text>
									</view>
									<view style="color:#666666;display: flex;align-items: center;font-size: 12px;">
										<text style="display: inline-block;color:#999999;width:40px;line-height: 20px;">库位</text>
										<text>{{cell.location}}</text>
									</view>
								</view>
								<view style="display:flex;align-items: center;justify-content: space-between;margin: 5px 0;height: 25px;">
									<view style="color:#FF4C4B;font-size:14px;">
										<text style="font-weight:900;">{{cell.kucun}}</text>
										<text style="display: inline-block;font-size:12px;color:#999999;margin-left:5px;">{{cell.unit_name}}</text>
									</view>
									<view class="button" :class="cell.shoopNum>0?'curr':''" @click="inputDialogToggle(item,cell)" style="text-align: center;font-size: 14px;">
										<text v-if="cell.shoopNum>0" style="border: 1px solid #f3a73f;color:#f3a73f;border-radius: 5px;padding: 0 5px;">+{{cell.shoopNum}}</text>
										<view v-else><uni-icons type="plus-filled" size="24"></uni-icons></view>
									</view>
								</view>
							</view>
						</view>
					</cp-goods-item>
				</cp-goods-select>
			</view>
			
			<view @click="toggle('bottom')" class="bottomBox">
				<text>
					<text>共<text style="margin: 0 3px;color: #FF4C4B;">{{Object.keys($store.state.orderlist).length}}</text>件商品</text>
					<text style="color: #ff4c4b;font-weight: 900;margin-left: 10px;">¥{{count==0?'0.00':count}}</text>
				</text>
				<text style="height: 100%;width:100px;display: flex;align-items: center;justify-content: center;border-radius: 5px;padding: 2px 10px;font-weight: normal;cursor: pointer;background: #2982FF;border-radius: 100px;color: #fff;" @click.stop="pushData">
					下一步
				</text>
			</view>

			<!-- 规格弹窗 -->
			<!-- <uni-popup ref="inputDialogGui" type="dialog">	
				<view style="width: 300px;border-radius: 11px;background-color: #fff;padding: 10px 15px;">
					<view style="font-size: 22px;font-weight: 900;margin-bottom: 10px;"></view>
					<view style="min-height: 100px;">
						<view v-for="(item,val) in specifications" :key="val+'_'" :class="'spe'+val+'_'">
							<view style="font-size: 16px;margin-bottom: 8px;">{{item.name}}</view>
							<view class="flexCenter" style="margin-bottom: 15px;">
								<text v-for="(ele,index) in item.type"
									:key="val+'_'+index"
									:class="['spe'+val+'_'+index,{'curr':item.status==index}]"
									@click="item.status=index"
									class="dia_goods__item">{{ele}}</text>
							</view>
						</view>
						
					</view>
					<view class="flexCenter" 
						style=" font-size: 18px; font-weight: 900; display: flex; justify-content: space-between; height: 35px; align-items: flex-end;">
						<view>总计<text style="margin: 0 3px;color: #1bd0fd;font-size: 22px;">99</text>元</view>
						<view style=" border-radius: 5px; padding: 2px 10px; font-size: 16px; font-weight: normal; cursor: pointer; background: rgb(27, 208, 253);"
						@click="toOrder"
						>+ 加入清单</view>
					</view>
					<uni-icons type="close" class="close-btn" size="40" @click="clsoeDialog"></uni-icons>
				</view>
				
			</uni-popup> -->

			<!-- 普通弹窗 上下左右 -->
			<uni-popup ref="popup" background-color="#fff" @change="change">
				<view class="popup-content" :class="{ 'popup-height': popupType === 'left' || popupType === 'right' }" style="max-height: 374px;overflow: auto;">
					<view style="font-size:18px;color:#333333;font-weight:900;line-height: 50px;text-align: center;">已选账单</view>
					<view v-for="(item,key) in zd" :key="key">
						<view class="flexCenter" style="justify-content: flex-start;align-items: center;padding: 10px;border-bottom: 1px solid #eee;">
							<view style="margin-right: 15px;" class="flexCenter radius5px"><img src="/static/images/200.png" alt="51" style="width:60px;height:60px;"></view>
							<view style="flex:1;font-size:18px;font-weight:900;margin-right: 10px;">
								<view>{{ item.name }}</view>
								<view style="color:#ff4c4b;font-size:13px;margin-top: 5px;">¥{{ item.price }}</view>
							</view>
							<view style="flex:0;">
								<view class="button-text" @click="inputDialogToggle('',item)" style="text-align: center;font-size: 14px;">
									<text style="border: 1px solid #f3a73f;color:#f3a73f;border-radius: 5px;padding: 0 5px;float: right;">+{{item.shoopNum}}</text>
								</view>
							</view>
						</view>
					</view>
				</view>
			</uni-popup>

			<!-- 数量修改弹窗 -->
			<uni-popup ref="inputDialog" type="dialog">
				<uni-popup-dialog ref="inputClose"  mode="input" title="输入数量价格" value="" placeholder="请输入内容" @confirm="dialogInputConfirm">
					<view style="padding: 20px 0;width: 80%;display: flex;align-items: center;">
						<label>数 量：</label>
						<input type="number" placeholder="请输入数量" v-model="shoopInputValue" style="box-sizing: border-box;width:78%;font-size: 14px;border: 1px #eee solid;height: 40px;padding: 0 10px;border-radius: 5px;color: #555;">
					</view>
					<view style="padding-bottom: 20px;width: 80%;display: flex;align-items: center;">
						<label>单 价：</label>
						<input type="digit" placeholder="请输入价格" v-model="shoopInputPrice" style="box-sizing: border-box;width:78%;font-size: 14px;border: 1px #eee solid;height: 40px;padding: 0 10px;border-radius: 5px;color: #555;">
					</view>
				</uni-popup-dialog>
			</uni-popup>
	</view>
</template>

<script>

	var _this;
	export default {
		data() {
			return {
				background: {
					// 渐变色
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				data:null,
				infodata:null,
				
				list: [],
				specifications:[],

				shoopInputValue:"",		// 商品数量vmodel
				shoopInputPrice:"",		// 商品数量vmodel
				count: 0,
				goodsObj: {},					// 选中的商品对象

				popupType:"",

				zd:{}

			}
		},
		onLoad() {
			_this = this;

			_this.getGoodsData();
		},
		watch: {
			shoopInputValue() {
				// console.log(this.shoopInputValue);
				this.$nextTick(() => {
					if(parseInt(this.shoopInputValue)>parseInt(this.goodsObj.kucun)) {
						this.shoopInputValue = this.goodsObj.kucun;
					}
				});
				
			}
		},
		methods: {
			getGoodsData() {
				_this._post_form('/api/goods/index', {}, (result) => {
					if(result.errno==0) {
						// result.data 对象格式 转为数组格式
						_this.list = Object.values(result.data);
					}
					this.list.forEach(element => {
						// - this.$set(原数组, 索引值, 需要赋的值)
						element.children.forEach(item => {
							this.$set(item, "shoopNum", 0);
							this.$set(item, "price", 0);
						})
					});
					console.log(this.list);
				});
			},
			clickLeft() {
				this.$store.state.orderlist = {};
				this.$store.state.selectAddress = {};
				uni.switchTab({url: "/pages/statistics/index"});
			},
			// 数量修改弹窗
			inputDialogToggle(classifi,goods) {
				this.goodsObj = goods;
				// console.log(this.$refs.inputDialog)
				// console.log(this.goodsObj);

				this.shoopInputValue = goods.shoopNum==0?"":goods.shoopNum;
				this.shoopInputPrice = goods.price==0?"":goods.price;
				this.$refs.inputDialog.open();
			},
			// 底部弹窗
			toggle(type) {

				// 获取对象长度
				if(Object.keys(this.$store.state.orderlist).length) {
					this.zd = this.$store.state.orderlist;
					console.log(this.zd);
					this.popupType = type;
					// open 方法传入参数 等同在 uni-popup 组件上绑定 type属性
					this.$refs.popup.open(type)
				}
				
			},
			change(e) {
				// console.log('当前模式：' + e.type + ',状态：' + e.show);
			},
			
			// 关闭弹窗
			clsoeDialog() {
				this.$refs.inputDialog.close()
			},
			orderlistCount() {
				this.count = 0;
				for (const key in this.$store.state.orderlist) {
					this.count += parseInt(this.$store.state.orderlist[key].shoopNum)*parseFloat(this.$store.state.orderlist[key].price).toFixed(2);
				}
				this.count = this.count.toFixed(2);
			},
			// 加入账单
			dialogInputConfirm(val) {
				if(this.shoopInputValue<=0) return uni.showToast({title: '请输入商品数量',icon: 'none',duration: 1000});
				if(this.shoopInputPrice==0) return uni.showToast({title: '请输入商品价格',icon: 'none',duration: 1000});
				uni.showLoading({mask:true,title: '加入账单中...'});


				setTimeout(() => {
					uni.hideLoading()
					this.goodsObj.shoopNum = this.shoopInputValue;
					this.goodsObj.price = this.shoopInputPrice;
					// this.$store.state.orderlist.push(this.goodsObj);
					this.$set(this.$store.state.orderlist, this.goodsObj.id, this.goodsObj);
					this.orderlistCount();
					// 关闭窗口后，恢复默认内容
					this.$refs.inputDialog.close();
					// console.log(this.goodsObj);
					// console.log(this.$store.state.orderlist);
				}, 300);
			},
			// 去到下单页
			pushData() {
				// console.log(this.$store.state.orderlist);
				// console.log(Object.keys(this.$store.state.inputDialog).length);
				// return
				// 获取对象长度
				let len = Object.keys(this.$store.state.orderlist).length;
				if(len==0) {
					uni.showToast({
						title: '未选择商品',
						icon: 'none'
					})
					// uni.showModal({
					// 		title: '提示',
					// 		content: '',
					// 		success: function (res) {
					// 				if (res.confirm) {
					// 						console.log('用户点击确定');
					// 				} else if (res.cancel) {
					// 						console.log('用户点击取消');
					// 				}
					// 		}
					// });
				} else {
					uni.showLoading({mask:true,title: '订单加载中...'});
					let s = setTimeout(() => {
						clearTimeout(s);
						uni.navigateTo({url: '/pages/shoppingCart/index'});
					}, 1000);
				}
			},

			// 加入账单
			toOrder(res) { },

			handelScrolltolower(e) {
				console.log('handelScrolltolower', e)
			},
			handelCategoryChange(e) {
				console.log('handelCategoryChange', e)
			}
		}
	}
</script>

<style lang="scss">
	// @import 'static/css/cp-goods.scss';

	.flexCenter {display: flex;}
	.radius5px {border-radius: 5px;overflow: hidden;}
	
	.cp-goods-select {
		display: grid;
		grid-template-columns: 194upx auto;
		height: 100%;
		width: 100%;
		&_wrap{
			height: 100%;
			width: 100%;
			overflow: auto;
		}
		&-aside {
			background-color: #fff;
			height: 100%;
		}
		/deep/ &-category {
			color: #696E83;
			background-color: #fff;
			height: calc(100% - 60upx);
			flex: 1;
			position: relative;
			
			&__wrap{
				position: relative;
			}
			
			&-item {
				width: 100%;
				padding: 12px 10px 11px 10px;
				box-sizing: border-box;
				position: relative;
				color: #666666;
				background: #F4F5F6;
				text-align: center;
				&:last-child{
					// margin-bottom: 200upx;
				}
			}
			
			&-item.actived {
				color: #3A3A3A;
				background-color: #fff;
				position: relative;
				color: #2982FF;
				font-weight: 900;
				font-size: 16px;
				&::before {
					content: "";
					display: block;
					width: 6upx;
					height: 100%;
					position: absolute;
					left: 0;
					top: 0;
					// background-color: #1660f5;
					background-color: transparent;
				}
			}
			
			&__actions{
				position: sticky;
				bottom: 0;
			}
		}
	// =========================================================
	
	
		/deep/ &-goods {
			background-color: transparent;
			height: 100%;
			box-sizing: border-box;
			
			&_placeholder{
				padding-bottom: 70vh;
			}
			
			&__classify {
				color: #333642;
				line-height: 85upx;
				position: sticky;
				top: 0px;
				z-index: 1;
				padding: 0 8px;
				font-size: 16px;
				font-weight: 900;
				// background-color: #F4F5F6;
				background-color: #fff;
				&_text{
					padding-left: 10upx;
				}
			}
			
			&__list{
				padding: 10upx;
				&.cloumn-2 ,&.cloumn-3{
					display: flex;
					flex-wrap: wrap;
				}
			}
			
			.cloumn-2 .goods__item {
				width: 49%;
				margin-left: 10upx;
				&:nth-of-type(2n+1){
					margin-left: 0;
				}
			}
			
			.cloumn-3 .goods__item{
				width: 32%;
				margin-left: 10upx;
				 &:nth-of-type(3n+1){
					margin-left: 0;
				}
			}
			
			.cp-goods-select-goods__list {
				padding: 8px;
				padding-top: 0;
			}
			
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
				.sub-info {
					font-size: 12px;
					color: #84898f;
				}
			}
		}
	}
	
</style>

<style lang="less">
	// /deep/ .uni-navbar__content  {height: 6vh;}
	// /deep/ .uni-nav-bar-text {font-size: 16px;}
	/deep/ .uni-dialog-content {padding: 0;flex-wrap: wrap;}
</style>

<style>
	.content {
		padding: 0;
		height: 100%;
		/* display: grid;
    	grid-template-rows: auto 60px; */
	}
	

	.bottomBox {
		width: 100%;
		height: 7vh;
		padding: 10px;
		padding-left: 20px;
		display: flex;justify-content: space-between;
		align-items: flex-end;background-color: rgb(255, 255, 255);
		box-shadow: rgb(204 204 204) 0px -1px 10px 0px;
		font-size: 16px;color: #666666;
		position: relative;z-index: 1;
		/* position: fixed;bottom: 0;z-index: 1; */
	}

	.grid-text {
		font-size: 28rpx;
		margin-top: 4rpx;
	}
	.dia_goods__item {
		cursor: pointer;
		min-width: 55px;
		height: 25px;
		line-height: 23px;
		text-align: center;
		border: 0.5px solid #1bd0fd;
		border-radius: 5px;
		display: block;
		margin-right: 12px;
	}
	.dia_goods__item.curr {
		background: #1bd0fd;
		color: #fff;
	}
	.close-btn {
		cursor: pointer;
		color: gray !important;
		position: absolute;
		bottom: -48px;
		left: 50%;
		margin-left: -25px;
	}
	.m_top15 {
		margin-top: 15px;
	}

	.m_top25 {
		margin-top: 25px;
	}

	.m_top5 {
		margin-top: 5px;
	}
	
	.u-grid-item-img {
		margin: 0 auto;
		display: block;
		padding: 5px 0;
		width: 28px;
		height: 28px;
	}
	
	
	.color-box {
		/* padding: 0 15px; */
		display: flex;
		align-items: center;
		justify-content: space-between;
		color: #fff;
		text-align: center;
		margin-top: 20rpx;
	}
	
	.color-item {
		display: flex;
		flex: 1;
		margin: 0 8rpx;
		flex-direction: column;
		border-radius: 6rpx;
		padding: 12rpx 0;
		height: 75px;
	}
	
	.bg1{
		background: linear-gradient(-125deg, #67c707, #a7f968);
	}
	
	.bg2{
		background: linear-gradient(-125deg, #16a8cc, #1bd0fd);
	}
	
	.bg3{
		background: linear-gradient(-125deg, #e29811, #f1cf71);
	}
	
	.bg4{
		background: linear-gradient(-125deg, #cc16b6, #d496d8);
	}
	
	.color-title {
		font-size: 30px;
		/* line-height: 75px; */
		height: 75px;
	}
	
	.color-id {
		font-size: 24rpx;
	}
	
	.text-area {
		display: flex;
		justify-content: center;
	}
	
	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	
	.item {
		padding: 10upx 30upx;
		background-color: #fff;
		margin: 0 10upx;
		margin-bottom: 10upx;
	}
	
	.num {
		border-radius: 50%;
		background-color: #ff0099;
		color: #fff;
		display: inline-block;
		width: 30upx;
		height: 30upx;
		line-height: 30upx;
		text-align: center;
		padding: 2px;
		position: absolute;
		top: 4px;
	}
	.uniui-plus-filled {color: #f3a73f !important;}
	
</style>
