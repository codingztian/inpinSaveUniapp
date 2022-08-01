<template>
	<view class="">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar height="6vh" statusBar=true shadow left-icon="left" title="库存预警" 
					color="#fff" background-color="rgb(60, 158, 253)"
					leftText="返回" @clickLeft="clickLeft"/>
			</view>
		</view>

		<!-- <view class="action"> -->
		<scroll-view  class="action scroll" scroll-y="true" @scrolltolower="lower()" >
			<view class="scrollView">
				<view class="flex header">
					<text class="title u-line-2 title1">商品名称</text>
					<text class="title u-line-2">库存</text>
					<text class="title u-line-2">状态</text>
					<text class="title u-line-2">预警数</text>

				</view>
				<view v-if="list.length">
					<view class="title-wrap" :index="index" v-for="(item, index) in list" :key="item.id">
						<text class="title u-line-2 title1">{{ item.goods_name }}</text>
						<text class="title u-line-2 e43d33">{{item.qty}}件</text>
						<text class="title u-line-2">
              <text :class="parseInt(item.qty)>parseInt(item.high)?'bgf3a73f':'bgFE4C4B'">
                {{
                  parseInt(item.qty)>parseInt(item.high)?'库存积压': '库存正常'
                }}
                <!-- parseInt(item.qty)<parseInt(item.low)?'库存不足':'库存正常' -->
              </text> 
            </text>
						<text class="title u-line-2">{{ item.high }}件</text>
					</view>
				</view>
			</view>
			<view>
				<uni-load-more iconType="auto" :status="status" v-if="lodingStatus" />
			</view>
		</scroll-view>
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				background: {
					backgroundImage: 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				list:[],
				show: false,
				
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
		methods: {
			clickLeft() {
				uni.navigateBack({delta: 1});
			},
			
			getlist() {
				_this._post_form('/api/order/dailyStat', {type:3}, (result) => {
					console.log(result);
					if(result.errno==0) {
						_this.list = result.data;
					}
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
			
		}
	}
</script>

<!-- page {background-color: #F4F5F6;} -->

<style lang="scss" scoped>
	.bc3711{color: #18bc37;}
	.f3a73f{color: #f3a73f;}
	.e43d33{color: #e43d33;}
	.f939c1{color: #8f939c;}
  .bgbc3711{background-color: #18bc37;color: #fff;padding: 2px 5px;border-radius: 2px;}
	.bgf3a73f{background-color: #f3a73f;color: #fff;padding: 2px 5px;border-radius: 2px;}
	.bgFE4C4B{background-color: #FE4C4B;color: #fff;padding: 2px 5px;border-radius: 2px;}
	.bgf939c1{background-color: #8f939c;color: #fff;padding: 2px 5px;border-radius: 2px;}
	.action {
		height:90vh;
		padding-top: 1vh;
		overflow: auto;
		box-sizing: border-box;
	}
	.action .scrollView {width:180vw;}
	.flex{display: flex;}
	.action .title-wrap {
		display: flex;
    min-height: 40px;
	}
	.action .title {
		text-align: center;
		width: 40vw;
		padding: 6px 10px;
		box-sizing: border-box;
		border-right: 1px solid #eee;
		border-bottom: 1px solid #eee;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 13px;
	}
	.action .title.title1 {
		width: 60vw;
		text-align: left;
		border-left: 1px solid #eee;
		background: #fff;
		position: sticky;
		left: 0;
		justify-content: flex-start;
		/*border-color: #d5d5d6!important;*/
	}
	.action .header {
		height: 50px;
		color: #999;
		border-top: 1px solid #eee;
		border-color: #d5d5d6;
		font-weight: 900;
		position: sticky;
		top: 0;
		background: #fff;
		z-index: 1;
		box-sizing: content-box;
	}
	.action .header .title {
		border-color: #d5d5d6;
		font-size: 15px;
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
 /deep/ .uni-scroll-view-content {overflow: auto;margin: 0 5px;box-sizing: border-box; }
</style>
