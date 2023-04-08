<template>
	<view class="user">
		<view class="top">
			<image src="../../static/images/history.png" mode=""></image>
			<view class="text"> 浏览历史 </view>
		</view>
		<view class="content">
			<view class="row" v-for="item in historyArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail(item)"/>
			</view>
		</view>
		<view class="nohistory" v-if="historyArr == []">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<view class="text">暂无浏览记录</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				historyArr:[]
			}
		},
		onShow(){
			// 获取缓存数据
			this.getHistory()
		},
		methods:{
			// 跳转到详情页
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			// 获取缓存数据
			getHistory(){
				this.historyArr = uni.getStorageSync("historyArr")
			}
		},
		updated(){
			console.log(1);
		}
	}
</script>

<style lang="scss">
.user{
	.top{
		padding:50rpx 0;
		background: #F8F8F8;
		color:#666;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 150rpx;
			height: 150rpx;
		}
		.text{
			font-size: 38rpx;		
			padding-top: 20rpx;
		}
	}
	.content{
		padding:30rpx;
		.row{
			border-bottom:1px dotted #efefef;
			padding:20rpx 0;
		}
	}
	.nohistory{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 450rpx;
		}
		.text{
			font-size: 26rpx;
			color:#888;
		}
	}
}
</style>
