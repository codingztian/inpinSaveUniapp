<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="订货清单" 
					color="#fff" background-color="rgb(60, 158, 253)" rightIcon="home-filled"
					@clickLeft="clickLeft" @clickRight="clickRight"/>
			</view>
		</view>

		<view style="padding: 20px 12px 10px;padding-bottom: 0;" :style="{height:selectAddressStatus?'20vh':'12vh'}" @tap="toUser">
			<view class="flexCenter" style="height:100%;display: grid;grid-template-columns: 40px auto 35px;background: #fff;border-radius: 5px;padding: 12px;">
				<view style="display: flex;align-items: center;"><uni-icons type="location-filled" size="30" style="color:#2590FF;"></uni-icons></view>
				<view style="display: flex;align-items: flex-start;flex-direction: column;justify-content: space-around;height: 100%;">
						<view v-if="selectAddressStatus" style="color:#333333;font-size: 17px;font-weight: 900;">{{selectAddress.name}}</view>
						<view v-if="selectAddressStatus" style="color:#999;">{{selectAddress.linkName}} {{selectAddress.mobile}}</view>
						<view v-if="selectAddressStatus" style="color:#666;">{{selectAddress.address}}</view>
						<view v-if="!selectAddressStatus"  style="color:#333333;font-size: 17px;font-weight: 900;">选择收货地址</view>
				</view>
				<view style="display: flex;align-items: center;"><uni-icons type="forward" size="30"></uni-icons></view>
			</view>
		</view>

		<view style="height: 70vh;padding: 12px;overflow: auto;">
			<view style="padding:12px;background: #fff;border-radius: 5px;">
				<view v-for="(item,key) in zd" :key="key">
					<view class="flexCenter" style="justify-content: flex-start;align-items: center;padding: 10px;border-bottom: 1px solid #eee;">
						<view style="margin-right: 15px;" class="flexCenter radius5px"><img :src="item.thumb_img" alt="51" style="width:60px;height:60px;"></view>
						<view style="flex:1;font-size:18px;font-weight:900;margin-right: 10px;">
							<view>{{ item.name }}</view>
							<view style="color:#ff4c4b;font-size:13px;margin-top: 5px;">¥{{ item.price }}</view>
						</view>
						<view style="flex:0;">
								<view class="button-text" style="text-align: center;font-size: 14px;">
									<text style="border: 1px solid #f3a73f;color:#f3a73f;border-radius: 5px;padding: 0 5px;float: right;">+{{item.shoopNum}}</text>
								</view>
						</view>
					</view>
				</view>
			</view>
		</view>

		<view style="width: 100%;height: 7vh;display: flex;justify-content: space-between;padding: 10px;align-items: flex-end;background-color: rgb(255, 255, 255);box-shadow: rgb(204 204 204) 0px -1px 10px 0px;padding-left: 20px;font-size: 16px;color: #666666;position: fixed;bottom: 0;z-index: 1;">
			<text>
				<text>共<text style="margin: 0 3px;color: #FF4C4B;">{{Object.keys(zd).length}}</text>件商品</text>
				<text style="color: #ff4c4b;font-weight: 900;margin-left: 10px;">¥{{count==0?'0.00':count}}</text>
			</text>

			<text @click="toOredrOver" style="height: 100%;width:100px;display: flex;align-items: center;justify-content: center;border-radius: 5px;padding: 2px 10px;font-weight: normal;cursor: pointer;background: #2982FF;border-radius: 100px;color: #fff;">
				提交
			</text>
		</view>

		<!-- 数量修改弹窗 -->
		<uni-popup ref="inputDialog" type="dialog">
			<uni-popup-dialog ref="inputClose"  mode="input" title="输入数量" value="" placeholder="请输入内容" @confirm="dialogInputConfirm">
				<input type="number" placeholder="请输入数量" v-model="shoopInputValue"
					style="width:100%;font-size: 14px;border: 1px #eee solid;height: 40px;padding: 0 10px;border-radius: 5px;color: #555;">
			</uni-popup-dialog>
		</uni-popup>

		
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				shoopInputValue:"",		// 商品数量vmodel
				goodsObj: {},					// 选中的商品对象
				selectAddressStatus: false,
				selectAddress: {},				// 选中的地址对象
				count: 0,

			}
		},
		computed:{
			zd() {	// vuex 数据页面初始化渲染 赋值到zd 否则为undefined
			    return this.$store.state.orderlist
			},
		},
		onLoad(e) {
			_this = this;
			console.log(this.$store.state.orderlist);
			console.log(this.$store.state.selectAddress.id);
			if(this.$store.state.selectAddress.id) {
				this.selectAddressStatus = true;
				this.selectAddress = this.$store.state.selectAddress
			}
			this.count = 0;
			for (const key in this.$store.state.orderlist) {
				this.count += parseInt(this.$store.state.orderlist[key].shoopNum)*parseFloat(this.$store.state.orderlist[key].price).toFixed(2);
			}
			this.count = this.count.toFixed(2);
		},
		methods: {
			clickLeft() {
				// uni.navigateTo({url: "/pages/overbooking/index"});
				uni.navigateBack({delta: 1});
			},
			clickRight() {
				uni.switchTab({
					url: "/pages/statistics/index"
				});
			},
			toUser() {
				uni.redirectTo({url: "/pages/shoppingCart/userinfo"});
			},
			// 数量修改弹窗
			inputDialogToggle(classifi,goods) {
				this.goodsObj = goods;
				// console.log(this.$refs.inputDialog)
				this.shoopInputValue = goods.shoopNum;
				this.$refs.inputDialog.open();
			},
			// 加入账单
			dialogInputConfirm(val) {

				uni.showLoading({title: '加入账单中'});

				setTimeout(() => {
					uni.hideLoading()
					console.log(this.goodsObj);
					this.goodsObj.shoopNum = this.shoopInputValue;
					// this.$store.state.orderlist.push(this.goodsObj);
					this.$set(this.$store.state.orderlist, this.goodsObj.id, this.goodsObj);
					
					// 关闭窗口后，恢复默认内容
					this.$refs.inputDialog.close()
				}, 100)
			},

			toOredrOver() {
				let goodsArr = []
				Object.values(this.zd).forEach(e=> {
					goodsArr.push({
						goods_id:e.id,
						goods_num:e.shoopNum,
						price:e.price,
					});
				});
				console.log(goodsArr);
				if(!this.selectAddress.id) {
					uni.showToast({title: '请选择收货地址', icon: 'none'});
					return;
				}
				uni.showLoading({mask:true,title: '提交账单中'});
				setTimeout(() => {
					_this._post_form('/api/order/sendorder', {
						goods_info: JSON.stringify(goodsArr),
						kehu_id: this.selectAddress.id,
					}, (result) => {
						uni.hideLoading();
						if(result.errno==0) {
							uni.navigateTo({url: '/pages/shoppingCart/over'});
						} else {
							uni.showToast({title: result.error, icon: 'none'});
						}
						
					});
					
				}, 1000)
			},
			
		}
	}
</script>

<style>
	page {background: #F4F5F6;}
	.flexCenter {display: flex;}
	.uni-icons.uniui-location-filled {color: #2590ff !important;}
</style>
