<template>
	<view class="">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" shadow left-icon="left" title="供应商" 
					color="#fff" background-color="rgb(60, 158, 253)"
					leftText="返回" @clickLeft="clickLeft" />
			</view>
		</view>

		<view class="action">
			<view>
				<view class="title-wrap" :index="index" v-for="(item, index) in unitlist" :key="item.id">
					<view>
						<text class="title u-line-2" style="color:#333;font-size: 18px;font-weight: 900;line-height: 32px;">{{ item.name }}</text>
						<text class="title u-line-2" style="color:#999999;font-size: 12px;">{{ item.linkName }} {{item.mobile}}</text>
						<text class="title u-line-2" style="color:#666666;font-size: 14px;">{{ item.address }}</text>
					</view>
					<view></view>
				</view>
			</view>
		</view>

		<view class="addBtnBox">
			<view class="addBtn" @click="clickRight">添加供应商</view>
		</view>
		
		<!-- <u-swipe-action :show="item.show" :index="index" v-for="(item, index) in unitlist" :key="item.id" @click="click" @open="open"
		 :options="options" >
			<view class="item u-border-bottom" @tap="contentclick(item.id)">
				<view class="title-wrap">
					<text class="title u-line-2">供应商编号：{{ item.code }}</text>
					<text class="title u-line-2">供应商名称：{{ item.name }}</text>
					<text class="title u-line-2">联系人：{{ item.contact }}</text>
					<text class="title u-line-2">联系方式：{{ item.phone }}</text>
					<text class="title u-line-2">账期(天)：{{ item.term }}</text>
				</view>
			</view>
		</u-swipe-action>
		<u-modal v-model="show" content="是否删除该供应商信息？" :show-cancel-button = "true" @confirm="confirm"></u-modal>
		<u-toast ref="uToast" /> -->

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
				unitlist:[],
				show: false,
				options: [
					{
						text: '删除',
						style: {
							backgroundColor: '#dd524d'
						}
					}
				],
				id:null,
				value: '',
				type: 'text',
				border: true
			}

		},
		onLoad() {
			_this = this;
			_this.getUnitList();
		},
		onNavigationBarButtonTap(e) {
			uni.navigateTo({
				url:'addsupplier'
			})
			
		},
		methods: {
			clickLeft() {
				uni.switchTab({url: '/pages/user/index'});
			},
			clickRight() {
				uni.navigateTo({url: '/pages/information/supplier/addsupplier'});
			},

			onNavigationBar(){
				uni.navigateTo({
					url:'addsupplier'
				})
			},
			getUnitList() {
				_this._post_form('/api/user/gys', {}, (result) => {
					console.log(result);
					_this.setData({'unitlist' : result.data.list})
				});
			},
			click(index, index1) {
				_this.show = true;
				_this.id = this.unitlist[index].id;
			},
			open(index) {
				this.unitlist[index].show = true;
			},
			confirm(){
				_this._post_form('/api/ykjp/information/basisinfo/supplier/deleteProduct', {
					id:_this.id
				}, (result) => {
					_this.getUnitList()
				});
			},
			contentclick(e){
				uni.navigateTo({
					url:'supplierdetails?id=' + e
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.action {
		height:86vh;
		// padding: 12px;
		overflow: auto;
	}
	.action .title-wrap {
		padding:12px 14px;
		// margin-bottom: 12px;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.action .title-wrap .title {
		font-size: 14px;
		color: #333;
		line-height: 24px;
	}
	.addBtnBox {
		height: 8vh;
		font-size: 20px;
		font-weight: 900;
		color: #fff;
		padding: 10px;
		letter-spacing: 1px;
	}
	.addBtn {
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		background: #2982FF;
		border-radius: 50px;
	}

	
</style>

<style lang="less">
	/deep/ .uni-navbar__content  {height: 6vh;}
	/deep/ .uni-nav-bar-text {font-size: 16px;}
</style>
