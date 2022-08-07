<template>
	<view class="content" v-if="list">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar shadow title="分类" statusBar=true height="6vh"
					color="#fff" background-color="rgb(60, 158, 253)" />
			</view>
		</view>

		<!-- 商品分类 -->
		<cp-goods-select
			height="90vh"  
			:props="{label:'name',value:'id',children:'children'}" 
			v-if="list"
			:options="list" 
			class="listlistlist"
			@scrolltolower="handelScrolltolower" 
			@category-change="handelCategoryChange">
			<cp-goods-item v-for="(item,index) in list" :key="index" :category="item.name" class="listlistlist">
				<view v-for="(cell,k) in item.children" :key="k" class="goods__item">
					<view class="flexCenter">
						<view style="margin-right: 15px;" class="flexCenter radius5px"><img :src="cell.thumb_img" alt="51" style="width:80px;height:80px;"></view>
					</view>
					<view style="border-bottom: 1px solid #EEEEEE;min-height: 110px;flex: 1;display: flex;flex-direction: column;justify-content: space-between;">
						<view>
							<view style="font-size:16px;color:#333333;font-weight:900;margin-bottom: 7px;">{{ cell.name }}</view>
							<view style="color:#666666;display: flex;font-size: 12px;min-height: 20px;">
								<text style="display: inline-block;color:#999999;width:40px;">供应商</text>
								<text style="flex: 1;">{{cell.factory_name}}</text>
							</view>
							<view style="color:#666666;display: flex;font-size: 12px;min-height: 20px;">
								<text style="display: inline-block;color:#999999;width:40px;">库位</text>
								<text style="flex: 1;">{{cell.location}}</text>
							</view>
						</view>
						<view style="display:flex;align-items: center;justify-content: space-between;margin: 5px 0px;">
							<view style="color:#FF4C4B;font-size:14px;">
								<text style="font-weight:900;">{{cell.kucun}}</text>
								<text style="display: inline-block;font-size:12px;color:#999999;margin-left:5px;">{{cell.unit_name}}</text>
							</view>
							<view style="display: flex;align-items: center;">
								<uni-icons type="phone-filled" size="22" style="margin-right:6px;" @tap="call(cell.manager.mobile,cell.manager.name)"></uni-icons>
							</view>
						</view>
					</view>
				</view>
			</cp-goods-item>
		</cp-goods-select>
		
		
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
				
			}
		},
		
		onShow() {
			_this = this;
			
			_this.getGoodsData();
		},
		methods: {
			call(phone,kg) {
				const res = uni.getSystemInfoSync();

				// ios系统默认有个模态框
				if(res.platform=='ios'){
					uni.makePhoneCall({
						phoneNumber:phone,
						success(){
							console.log('拨打成功了');
						},
						fail() {
							console.log('拨打失败了');
						}
					})
				}else{
					//安卓手机手动设置一个showActionSheet
					uni.showActionSheet({
						itemList: [kg+'：'+phone,'呼叫库管'],
						success:function(res){
							console.log(res);
							if(res.tapIndex==1){
								uni.makePhoneCall({
									phoneNumber: phone,
								})
							}
						}
					})
				}
			},
			getGoodsData() {
				_this._post_form('/api/goods/index', {}, (result) => {
					console.log(result);
					if(result.errno==0) {
						// result.data 对象格式 转为数组格式
						_this.list = Object.values(result.data);
					}
					console.log(_this.list);

					// _this.data = result.data
				});
			},
			handelScrolltolower(e) {
				console.log('handelScrolltolower', e)
			},
			handelCategoryChange(e) {
				console.log('handelCategoryChange', e)
			}
		}
	}
</script>

<style lang="less">
	// /deep/ .uni-navbar__content  {height: 6vh;}
	// /deep/ .uni-nav-bar-text {font-size: 16px;}
</style>

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
				color: #1660f5;
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
					padding-top: 0px;
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

<style>
	.content {
		padding: 0;
	}

	.grid-text {
		font-size: 28rpx;
		margin-top: 4rpx;
		color: $u-type-info;
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
	
	.color-value {
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
	.uniui-phone-filled {color: #ccc !important;}
	
</style>
