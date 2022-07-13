<template>
	<view class="">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" shadow left-icon="left" title="客户信息" 
					color="#fff" background-color="rgb(60, 158, 253)"
					leftText="返回" 
					@clickLeft="clickLeft"/>
			</view>
		</view>

		<view class="action">
			<view>
				<view class="title-wrap" :index="index" v-for="(item, index) in list" :key="item.id">
					<view>
						<text class="title u-line-2">{{ item.cLevelName }}</text>
						<text class="title u-line-2">{{ item.name }} {{ item.number }}</text>
						<text class="title u-line-2">{{ item.cCategoryName }}</text>
					</view>
					<view>
						<text @tap="contentclick(item.id)">
							<uni-icons type="compose" size="24"></uni-icons>
						</text>
						<!-- <text @open="open(id)">
							<uni-icons type="trash" size="24"></uni-icons>
						</text> -->
					</view>
				</view>
			</view>
		</view>
		<view class="addBtnBox">
			<view class="addBtn" @click="clickRight">添加客户</view>
		</view>
		
		<u-modal v-model="show" content="是否删除该客户？" :show-cancel-button = "true" @confirm="confirm"></u-modal>
		<u-toast ref="uToast" />
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
				list:[],
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
			_this.getlist();
		},
		onNavigationBarButtonTap(e) {
			uni.navigateTo({
				url:'add'
			})
			
		},
		methods: {
			clickLeft() {
				uni.switchTab({url: '/pages/user/index'});
			},
			clickRight() {
				uni.navigateTo({url: '/pages/information/customerinfo/add'});
			},

			onNavigationBar(){
				uni.navigateTo({
					url:'add'
				})
			},
			getlist() {
				_this._post_form('/api/user/kehu', {}, (result) => {
					console.log(result);
					_this.setData({'list' : result.data.list})
				});
			},
			click(index, index1) {
				_this.show = true;
				_this.id = this.list[index].id;
			},
			open(index) {
				this.list[index].show = true;
			},
			confirm(){
				_this._post_form('/api/ykjp/information/basisinfo/customerinfo/del', {
					id:_this.id
				}, (result) => {
					_this.getlist()
				});
			},
			contentclick(e){
				uni.navigateTo({url: '/pages/information/customerinfo/details?id=' + e});
			}
		}
	}
</script>

<!-- page {background-color: #F4F5F6;} -->

<style lang="scss" scoped>
	.action {
		height:86vh;
		padding: 12px;
		overflow: auto;
	}
	.action .title-wrap {
		margin-bottom: 12px;
		display: flex;
		align-items: center;
    	justify-content: space-between;
	}

	.action .title-wrap .title {
		font-size: 14px;
		color: #333;
		line-height: 30px;
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
