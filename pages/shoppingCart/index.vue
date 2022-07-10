<template>
	<view class="content">
		<view class="flexCenter" style="height:100px;display: grid;grid-template-columns: 40px auto 35px;background: #fff;border-radius: 5px;padding: 12px;margin-bottom: 20px;margin: 12px;">
			<view style=""><uni-icons type="location-filled" size="30" style="color:#2590FF;"></uni-icons></view>
			<view style="display: flex;align-items: flex-start;flex-direction: column;justify-content: space-around;height: 76px;">
				<view style="color:#333333;font-size: 17px;font-weight: 900;">华新生鲜大卖场</view>
				<view style="color:#999;">张三 13207196768</view>
				<view style="color:#666;">高新大道华新大厦A座11栋</view>
			</view>
			<view style="display: flex;align-items: center;"><uni-icons type="forward" size="30"></uni-icons></view>
		</view>

		<view style="margin: 12px;padding:12px;background: #fff;border-radius: 5px;">
			<view v-for="(item,key) in zd" :key="key">
			<view class="flexCenter" style="justify-content: flex-start;align-items: center;">
				<view style="margin-right: 15px;" class="flexCenter radius5px"><img src="/static/images/200.png" alt="51" style="width:60px;height:60px;"></view>
				<view style="flex:1;">{{ item.label }}</view>
				<view style="flex:1;">
						<view class="button-text" @click="inputDialogToggle('',item)" style="min-width: 52px;padding: 0 10px;border: 1px solid #BBBBBB;border-radius: 5px;text-align: center;height: 28px;line-height: 28px;font-size: 14px;">
							{{ item.shoopNum }}
						</view>
				</view>
			</view>
		</view>
		</view>

		<view style="width: 100%;height: 60px;display: flex;justify-content: space-between;padding: 10px;align-items: flex-end;background-color: rgb(255, 255, 255);box-shadow: rgb(204 204 204) 0px -1px 10px 0px;padding-left: 20px;font-size: 16px;color: #666666;position: fixed;bottom: 0;z-index: 1;">
			<text>共<text style="margin: 0 3px;color: #FF4C4B;">{{Object.keys(zd).length}}</text>件</text>
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

			}
		},
		computed:{
			zd() {
			    return this.$store.state.orderlist
			},
		},
		onLoad() {
			_this = this;
			console.log(this.$store.state.orderlist)
		},
		methods: {
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
				uni.showLoading({title: '提交账单中'});
				setTimeout(() => {
					uni.hideLoading()
					uni.navigateTo({
						url: '/pages/shoppingCart/over'
					})
				}, 300)
			},
			
		}
	}
</script>

<style>
	page {background: #F4F5F6;}
	.flexCenter {display: flex;}
	
</style>
