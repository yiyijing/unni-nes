<template>
	<view class="detail">
		<view class="title">{{detail.title}}</view>
		<view class="info">
			<view class="author">编辑：{{detail.author}}</view>
			<view class="time">发布日期：{{detail.posttime}}</view>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		<view class="description">
			声明：本站的内容均采集与腾讯新闻，请联系QQ2563616454进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
		</view>
	</view>
</template>

<script>
	import { parseTime } from '@/utils/tool.js'
	export default {
		data() {
			return {
				detail:{}
			};
		},
		onLoad(options){
			this.getDetail(options)
		},
		methods:{
			// 获取数据信息
			getDetail(options){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data:options,
					success: (res) => {
						res.data.content=res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
						res.data.posttime = parseTime(res.data.posttime)
						this.detail = res.data
						// 缓存数据
						this.saveHistory()
						// 设置页面标题
						uni.setNavigationBarTitle({
							title:this.detail.title
						})
					}
				})
			},
			// 保存缓存
			saveHistory(){
				let historyArr = uni.getStorageSync("historyArr") || [];
				let {id,classid,picurl,title} = this.detail
				let item = {
					id,
					classid,
					picurl,
					title,
					looktime:parseTime(Date.now())
				}
				// 去重
				let index = historyArr.findIndex(i=>{
					return i.id==this.detail.id&&i.classid==this.detail.classid
				})
				if(index >= 0) {
					historyArr.splice(index,1)
				}
				historyArr.unshift(item)
				historyArr.splice(0,10)
				uni.setStorageSync("historyArr",historyArr)
			}
		},
	}
</script>

<style lang="scss">
.detail{
	padding:30rpx;
	.title{
		font-size: 46rpx;
		color:#333;
	}
	.info{
		background: #F6F6F6;
		padding:20rpx;
		font-size: 25rpx;
		color:#666;
		display: flex;
		justify-content: space-between;
		margin:40rpx 0;
	}
	.content{
		padding-bottom:50rpx;
	}
	.description{
		background: #FEF0F0;
		font-size: 26rpx;
		padding:20rpx;
		color:#F89898;
		line-height: 1.8em;
	}
}
</style>

