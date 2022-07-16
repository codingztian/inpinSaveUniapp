<template>
	<view class="content">
		<view class="box-bg">
			<view class="box-bg uni-nav-bar">
				<uni-nav-bar statusBar=true height="6vh" shadow title="首页"
					color="#fff" background-color="rgb(60, 158, 253)" />
			</view>
		</view>
		
		<view class="contentCon">
			<!-- <view class="yjInfo">
				<text class="icon"></text>
				<text style="color:#4F5B94;font-size:13px;">您有120单待审核，请及时审批。</text>
			</view> -->
			<view class="color-box" v-if="infodata">
				<view class="color-item ibg1" @click="toOverbooking('inOrder')">
					<view class="color-title">入库管理</view>
					<view class="color-value">
						今日入库：{{ parseInt(infodata.SALE) }}
					</view>
					<!-- <view class="color-value">
						待审核：{{ parseInt(infodata.SALE) }}
					</view> -->
				</view>
				<view class="color-item ibg2" @click="toOverbooking('outOrder')">
					<view class="color-title">出库管理</view>
					<view class="color-value">
						今日出库：{{ parseInt(infodata.PUR) }}
					</view>
					<!-- <view class="color-value">
						待审核：{{ parseInt(infodata.PUR) }}
					</view> -->
				</view>
			</view>
			
			<view class="color-box" v-if="infodata">
				<view class="color-item ibg3" style="display: flex;flex-direction: column;justify-content: center;">
					<view class="color-title" style="text-align: center;">
						<!-- 库存预警 -->
						敬请期待
					</view>
					<!-- <view class="color-value">
						预警商品数：{{ parseInt(infodata.PUR) }}
					</view>
					<view class="color-value">
						最低库存数：{{ parseInt(infodata.PUR) }}
					</view> -->
				</view>
				<view class="color-item ibg4">
					<view class="color-title">联系库管</view>
					<view class="color-value">{{ kginfo.manager_name }}</view>
					<view class="color-value">{{ kginfo.manager_mobile }}</view>
				</view>
			</view>
			
			<view>
				<view class="akeySend bg2" @click="toOverbooking('overbooking')">
					<view style="font-size:18px;font-weight: 900;">快捷发货</view>
					<view>选择商品和客户，库管为你快捷发货</view>
				</view>
			</view>
			<!-- <uni-card><text>这是一个基础卡片示例，内容较少，此示例展示了一个没有任何属性不带阴影的卡片。</text></uni-card> -->
				
			<!-- <view class="m_top25" v-for="(listitem, index) in data.menus" :key="index" >
				<u-section  :title="listitem.title" :right="false" :show-line="true"></u-section>
				<view class="white m_top5">
					<u-grid :col="3"  :border="false">
						<u-grid-item v-for="(item, idx) in listitem.info" :key="idx" @tap="onClick(item.url?item.url:null)">
							<image class="u-grid-item-img" :src="item.src"></image>
							<view class="grid-text">{{item.title}}</view>
						</u-grid-item>
					</u-grid>
				</view>
			</view> -->
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
				data:null,
				infodata: {},
				kginfo:{}
			}
		},
		onLoad() {
			_this = this;
			_this.getinfo();
		},
		methods: {
			toOverbooking(path) {
				uni.navigateTo({url: '/pages/'+path+'/index'})
			},

			onClick(url){
				if (url) {
					uni.navigateTo({
						url:url
					})
				}
			},
			getinfo(){
				_this._post_form('/api/stat/home', {}, (result) => {
					console.log(result);
					_this.infodata = result.data
				});
				_this._post_form('/api/user/manager', {}, (result) => {
					_this.kginfo = result.data;
				});
				
				// _this._post_form('api/ykjp/summary/Purchase/index', {}, (result) => {
				// 	_this.infodata = result.data.data
				// });
			}
		}
	}
</script>

<style lang="less">
	// /deep/ .uni-navbar__content  {height: 6vh;}
	// /deep/ .uni-nav-bar-text {font-size: 20px;}
</style>

<style>
	.contentCon {
		height: 90vh;
		padding: 10px 10px;
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
		flex: 1;
		margin: 0 8rpx;
		padding: 12rpx 0;
		height: 103px;
		border-radius: 10px;
	}
	.color-title {
		padding: 12px;
		font-size: 18px;
		width: 100%;
		text-align: left;
	}
	
	.color-value {
		font-size: 13px;
    text-align: left;
    letter-spacing: 0px;
    padding-left: 12px;
		line-height: 20px;
	}
	
	.akeySend {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-start;
		height: 72px;
    padding: 12px;
    font-size: 14px;
    margin: 10px 4px;
		border-radius: 8px;
    color: #fff;
	}
	.akeySend:after {border: none;}

	.ibg1 {background: url(../../static/images/ck.png) center / cover no-repeat;}
	.ibg2 {background: url(../../static/images/rk.png) center / cover no-repeat;}
	.ibg3 {background: url(../../static/images/yj.png) center / cover no-repeat;}
	.ibg4 {background: url(../../static/images/kg.png) center / cover no-repeat;}
	.yjInfo {
		display: flex;
    justify-content: flex-start;
    align-items: center;
    background: #E4F4FF;
    border-radius: 5px;
    margin: 0 4px;
		height: 40px;
	}
	.yjInfo .icon {
		width: 24px;
		height: 24px;
		margin: 0 12px;
		background: url(../../static/images/tabbar/tig.png);
	}
	

		
	.bg1{
		background: linear-gradient(-125deg, #67c707, #a7f968);
	}
	.bg2{
		background: linear-gradient(-125deg, #70C1FD, #399BFD);
	}
	.bg3{
		background: linear-gradient(-125deg, #e29811, #f1cf71);
	}
	
	.bg4{
		background: linear-gradient(-125deg, #cc16b6, #d496d8);
	}
</style>
