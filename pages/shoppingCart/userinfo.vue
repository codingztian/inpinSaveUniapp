<template>
	<view class="">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="客户地址" 
					color="#fff" background-color="rgb(60, 158, 253)"
					leftText="返回" 
					@clickLeft="clickLeft"/>
			</view>
		</view>

			<scroll-view  class="action scroll" scroll-y="true" @scrolltolower="lower()" >
				<view style="padding: 12px;">
					<view class="title-wrap" :class="{'curr': wrapIndex==index}" @tap="currTap(item,index)" :index="index" v-for="(item, index) in list" :key="item.id">
						<view>
							<text class="title u-line-2" style="color:#333;font-size: 18px;font-weight: 900;line-height: 32px;">{{ item.name }}</text>
							<text class="title u-line-2" style="color:#999999;font-size: 12px;">{{ item.linkName }} {{item.mobile}}</text>
							<text class="title u-line-2" style="color:#666666;font-size: 14px;">{{ item.address }}</text>
						</view>
						<view style="display:flex;">
							<view class="check">
								<view class="nk"></view>
							</view>
						</view>
					</view>
				</view>
				<view>
					<uni-load-more iconType="auto" :status="status" v-if="lodingStatus" />
				</view>
			</scroll-view>
		<view class="addBtnBox">
			<view class="addBtn" @click="toOrder">确认收获地址</view>
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
				list:[],
				show: false,

				id:null,
				value: '',
				type: 'text',
				border: true,
				selectObj:{},

				page:1,
				pageinfo:{},
				status: 'loading',
				lodingStatus: false,
				wrapIndex: -1,
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
				uni.navigateTo({url: '/pages/shoppingCart/index'});
			},
			currTap(item,e) {
				console.log(item);
				this.wrapIndex = e;
				this.selectObj = item;
			},
			toOrder() {
				this.$store.state.selectAddress = this.selectObj;
				uni.navigateTo({
					url:'/pages/shoppingCart/index'
				})
			},
			getlist() {
				_this._post_form('/api/user/kehu', {page:this.page}, (result) => {
					console.log(result);
					// _this.setData({'list' : result.data.list})
					_this.pageinfo = result.data.pageinfo;
					_this.list = _this.list.concat(result.data.list);
				});
			},

			lower(e) {
				if(this.pageinfo.count < 10) return false;
				if(this.lodingStatus) return false;
				this.lodingStatus = true;
				let set = setTimeout(() => {
					clearTimeout(set);
					this.page++;
					this.lodingStatus = false;
					this.getlist()
				}, 3000)
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
		width: 100%;
		padding-bottom: 8px;
		display: flex;
		align-items: center;
    	justify-content: space-between;
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

	.check {
		width: 22px;
    height: 22px;
    border: 1px solid #ccc;
    border-radius: 20px;
		display: flex;
    align-items: center;
    justify-content: center;
	}
	.check .nk {
		width: 13px;
		height: 13px;
		border-radius: 10px;
		background: #fff;
	}
	.curr .check {
		border-color: #f3a73f;
	}
	.curr .check .nk {
		background: #f3a73f;
	}
	
</style>

<style lang="less">

</style>
