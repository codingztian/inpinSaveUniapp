<template>
	<view class="">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="客户信息" 
					color="#fff" background-color="rgb(60, 158, 253)"
					leftText="返回" 
					@clickLeft="clickLeft"/>
			</view>
		</view>

			<!-- <view class="action"> -->
				<scroll-view  class="action scroll" scroll-y="true" @scrolltolower="lower()" >
					<view style="padding: 12px;" v-if="list.length">
						<view class="title-wrap" :index="index" v-for="(item, index) in list" :key="item.id">
							<view>
								<text class="title u-line-2" style="color:#333;font-size: 18px;font-weight: 900;line-height: 32px;">{{ item.name }}</text>
								<text class="title u-line-2" style="color:#999999;font-size: 12px;">{{ item.linkName }} {{item.mobile}}</text>
								<text class="title u-line-2" style="color:#666666;font-size: 14px;">{{ item.address }}</text>
							</view>
							<view style="display:flex;">
								<view @tap="eidtorUser(item)">
									<uni-icons type="compose" size="24"></uni-icons>
								</view>
								<view @tap="delUser(item.id)" style="margin-left:10px;">
									<uni-icons type="trash-filled" size="24"></uni-icons>
								</view>
								<!-- <text @open="open(id)">
									<uni-icons type="trash" size="24"></uni-icons>
								</text> -->
							</view>
						</view>
					</view>
					<view>
						<uni-load-more iconType="auto" :status="status" v-if="lodingStatus" />
					</view>
				</scroll-view>
			
			<!-- </view> -->

		<view class="addBtnBox">
			<view class="addBtn" @click="addUser">添加客户</view>
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
				border: true,

				page:1,
				pageinfo:{},
				status: 'loading',
				lodingStatus: false,
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
			addUser() {
				uni.navigateTo({url: '/pages/information/customerinfo/add'});
			},
			eidtorUser(e) {
				uni.navigateTo({url: '/pages/information/customerinfo/add?detailDate='+ JSON.stringify(e)});
			},
			delUser(e) {
				_this.show = true;
				_this.id = e
			},
			confirm(){
				_this._post_form('/api/user/kehudo', {
					action: "delete",
					id: _this.id
				}, (result) => {
					console.log(result);
					_this.$refs.uToast.show({
						title: '删除成功',
						type: 'success',
					});
					_this.getlist()
				});
			},
			getlist() {
				_this._post_form('/api/user/kehu', {page:this.page}, (result) => {
					console.log(result);
					// _this.setData({'list' : result.data.list})
					_this.pageinfo = result.data.pageinfo;
					_this.list = _this.list.concat(result.data.list);
					console.log(_this.list);
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
					this.getlist()
				}, 3000)
			},


			onNavigationBar(){
				uni.navigateTo({
					url:'add'
				})
			},
			clickShow(index, index1) {
				_this.show = true;
				_this.id = this.list[index].id;
			},
			open(index) {
				this.list[index].show = true;
			},
			
		}
	}
</script>

<!-- page {background-color: #F4F5F6;} -->

<style lang="scss" scoped>
	.action {
		height:82vh;
		overflow: auto;
	}
	.action .title-wrap {
		padding-bottom: 5px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: 1px solid #eee;
	}

	.action .title-wrap .title {
		font-size: 14px;
		color: #333;
		line-height: 26px;
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
	.uni-icons {color: #999 !important;}
	
</style>

<style lang="less">

</style>
