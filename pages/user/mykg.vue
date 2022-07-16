<template>
	<view class="">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="我的库管" 
					color="#fff" background-color="rgb(60, 158, 253)"
					leftText="返回" @clickLeft="clickLeft" />
			</view>
		</view>

		<view class="action">
			<view>
				<view class="title-wrap" :index="key" v-for="(item, key) in unitlist" :key="item.id">
					<view class="userinfo">
						<view class="inview">
							<text>库管名称</text>
							<text>{{item}}</text>
						</view>
						<view class="inview">
							<text>联系方式</text>
							<text>{{item}}</text>
						</view>
					</view>
				</view>
				<!-- <view class="addBtnBox">
					<view class="addBtn" @click="clickRight">添加库管</view>
				</view> -->
			</view>
		</view>
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
			getUnitList() {
				_this._post_form('/api/user/manager', {}, (result) => {
					_this.unitlist = result.data;
					console.log(_this.unitlist);
				});
			},

			onNavigationBar(){
				uni.navigateTo({
					url:'addsupplier'
				})
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
	.userinfo {
		width: 100%;
		font-size: 18px;
    font-weight: 900;
    line-height: 35px;
    padding: 0 15px;
	}
	.userinfo .inview {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 10px;
		border-bottom: 1px solid #e5e5e5;
	}
	.action {
		height:90vh;
		// padding: 12px;
		overflow: auto;
	}
	.action .title-wrap {
		// padding:12px 14px;
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

</style>
